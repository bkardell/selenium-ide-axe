# selenium-ide-axe
A selenium ide axe extension


Sometimes I use Selenium IDE for writing simple tests because, why not.  You can teach anyone to use it, it's free, tests are easily sharable, importable, etc.  As a developer sometimes it's just easy.  Recently I came across a situation where someone wanted to use  [dequeue's axe-core](https://github.com/dequelabs/axe-core) for writing doing some testing - the trouble that the relevant infrastructure and setup for what we really want ultimately isn't going to be available for a while.  In the interim we have tools like google's excellent a11y audit - but this means we have to do a lot manually.  It'd be nice if we could script the setting up of scenarios as we go so that any time, people could just run the suite and say "oh, whoops, that thing broke".  I thought "too bad I can't just use it from selenium ide so that we could get started".  Then I thought, "Wait... can I?"  The answer is "kind of yes".

Selenium IDE has a plugin/extensions architecture and you can write new extensions.  Just to get the ball rolling I whipped this up quickly as a proof of concept.  Here's how you use it.

Once you have Selenium IDE installed (it's just an add-on for Firefox), you launch it and in the menu go to "Options > Options" and you will see a field for "Selenium Core Extensions (user-extensions.js)".  You pop this file in there and then when you are writing your Selenium tests you have some options...


```javascript
/* 
   Runs axe.a11yCheck on the element matching the locator.  It will store the result 
   for later (it's async, don't forget to pause after so it will happen.. if someone can 
  help me make this better please do!).  You can optionally name this, or it will just get 
  a generic name.  No assertions happen here.
*/
checkA11yAndStore(locator, ?name)

/* 
  Simply asserts that there are no violations in the named report (or the last one)
  locator is just 'empty' in Selenium IDE, it's unused - that's just how the pattern works.
  If the assert fails, processing of this test stops. You can see the serialized JSON result 
  from axe in the log.
*/
verifyA11yReport(locator, name)


/* 
  Since it is helpful to not stop processing the rest of the tests and since those logs are 
  hard to read, yo? you can wait until the end and then call this - it will serialize a little
  HTML report for you into the browser window it is controlling..
*/
a11yReportDump(locator, name)
```
