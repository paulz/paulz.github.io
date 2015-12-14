# Setting Up LiveReload With Jekyll
# JANUARY 18, 2014
# http://dan.doezema.com/2014/01/setting-up-livereload-with-jekyll/
#
# 1. guard
# 2. switch to the browser, make sure the LiveReload extension is turned on, and navigate to a blog post.
# 3. Open up the blog post in an editor, modify it and save the changes. When you switch back to the browser you should find that the page has automatically reloaded.

guard 'jekyll-plus', :serve => true do
  watch /.*/
  ignore /^_site/
end

guard 'livereload' do
  watch /.*/
end
