---
layout: post
title:  "Testing code that catches exceptions"
date:   2015-11-14 16:32:39
categories: ios tdd unit objc
---
In Objective-C throwing `NSException` is a bad way to control flow of the program. Usually we want to return a boolean result and set `NSError` to indicate there was an error.

Sometimes we need to write code that catches exceptions thrown from a framework or third party library we have no control.

Take for example `-[NSFileHandle seekToFileOffset:]`

Apple Documentation for [seekToFileOffset](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSFileHandle_Class/#//apple_ref/occ/instm/NSFileHandle/seekToEndOfFile) has this warning:

{% highlight text %}
Special Considerations
Raises an exception if the message is sent to an NSFileHandle object representing a pipe or socket, if the file descriptor is closed, or if any other error occurs in seeking.
{% endhighlight %}

So the code that uses seek to file offset has to be wrapped into `@try` block. And if we want to report the error we have to have a `@catch` block.

Here is an example of code that uses `seekToFileOffset` with `@try/@catch`

{% highlight objc %}
- (BOOL)updateFileAtURL:(NSURL *)fileUrl error:(NSError **)error {
  NSFileHandle *file = [NSFileHandle fileHandleForUpdatingURL:fileUrl error:error];
  @try {
    [file seekToFileOffset:123];        // Debugger stops at this line throwing an expected exception in test
  } @catch (NSException *exception) {
    *error = [NSError errorWithDomain:@"FileUpdateErrorDomain"
                                 code:-1
                             userInfo:@{@"exception" : exception}];
    return FALSE;
  }
  [file writeData:[self dataToWrite]];
  return TRUE;
}
end
{% endhighlight %}

Here is a test that verifies the behavior when an exception is caught:
{% highlight objc %}
it(@"should report exception", ^{
    NSError *error = nil;
    [[theValue([updateFileAtURL:faultyFileUrl error:&error]) should] equal:theValue(NO)];
    [[error.domain should] equal:@"FileUpdateErrorDomain"];

    NSException *illegalSeek = error.userInfo[@"exception"];
    [[illegalSeek.name should] equal:NSFileHandleOperationException];
    [[illegalSeek.reason should] equal:@"*** -[NSConcreteFileHandle seekToFileOffset:]: Illegal seek"];
});
{% endhighlight %}


The challenge becomes running those tests in debugger.
If you have Xcode [Exception Breakpoint](https://developer.apple.com/library/ios/recipes/xcode_help-breakpoint_navigator/articles/adding_an_exception_breakpoint.html) set in debugger then debugger going to stop on that test every time.
If you do not have exceptions enabled then debugger won't catch other unexpected exceptions during test run.

I used to run tests two times once with exceptions disabled and if there is a failure in a test second time with enabled exceptions to debug the problem.
One the second run I had to tell debugger to continue every time it stopped on expected exceptions until I get to the test failure.
It would be nice if we can tell debugger to skip expected exceptions.

Here is a debugger command you can add to the Exception Breakpoint that will continue execution when special global variable `debuggerContinueOnExceptions` is set to TRUE:

{% highlight python %}
script lldb.process.Continue() if lldb.frame.EvaluateExpression("debuggerContinueOnExceptions").GetValueAsUnsigned() else None
{% endhighlight %}

My `SpecHelper.h` defines it as 
{% highlight objc %}
extern BOOL debuggerContinueOnExceptions;
{% endhighlight %}

and sets it to NO within `SpecHelper.m`
{% highlight objc %}
BOOL debuggerContinueOnExceptions = NO;
{% endhighlight %}

Now any test that expects the exception can set it to YES before the test and restore it to NO after.

{% highlight objc %}
beforeEach(^{
    debuggerContinueOnExceptions = YES;
});

afterEach(^{
    debuggerContinueOnExceptions = NO;
});

{% endhighlight %}

With this change I can always have exceptions breakpoint enabled and run tests in debugger. When an unexpected is exception happens debugger stops and let me investigate the problem.

Complete example project with shared exception breakpoint can be found here: [https://github.com/paulz/XcodeDebuggerContinueOnExceptions](https://github.com/paulz/XcodeDebuggerContinueOnExceptions)
