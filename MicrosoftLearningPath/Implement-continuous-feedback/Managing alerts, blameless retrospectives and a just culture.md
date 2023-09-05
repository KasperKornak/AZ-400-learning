# Managing alerts
## Examine when get a notification
Application Insights automatically analyzes the performance of your web application and can warn you about potential problems. You might be reading it because you received one of our smart detection notifications.

Application Insights has detected that the performance of your application has degraded in one of these ways:

- **Response time degradation**: Your app has started responding to requests more slowly than it used to. The change might have been rapid, for example, because there was a regression in your latest deployment. Or it might have been gradual, maybe caused by a memory leak.
- **Dependency duration degradation**: Your app makes calls to a REST API, database, or other dependencies. The dependency is responding more slowly than it used to.
- **Slow performance pattern**: Your app appears to have a performance issue that is affecting only some requests. For example, pages are loading more slowly on one type of browser than others; or requests are being served more slowly from one server. Currently, our algorithms look at page load times, request response times, and dependency response times.

## Improving performance
### Triage
**First, does it matter?** If a page is always slow to load, but only 1% of your site's users ever have to look at it, maybe you have more important things to think about. On the other hand, if only 1% of users open it, but it throws exceptions every time, that might be worth investigating. Use the **impact statement (affected users or % of traffic)** as a general guide but be aware that it isn't the whole story.

### Diagnose slow page loads
**Where is the problem?** Is the server slow to respond, is the page long, or does the browser have to do much work to display it? Open the Browsers metric blade. The segmented display of browser page load time shows where the time is going.

- If **Send Request Time is high**, the server responds slowly, or the request is a post with much data. Look at the performance metrics to investigate response times.
- Set up dependency tracking to see whether the slowness is because of external services or your database.
- If **Receiving Response is predominant**, your page and its dependent parts - JavaScript, CSS, images, and so on (but not asynchronously loaded data) are long. Set up an availability test and be sure to set the option to load dependent parts. When you get some results, open the detail of a result, and expand it to see the load times of different files.
- **High Client Processing time** suggests scripts are running slowly. If the reason isn't clear, consider adding some timing code and sending the times in track metrics calls.