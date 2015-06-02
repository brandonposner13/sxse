# Installation #
Download the application from [here](http://sxse.googlecode.com/files/sxse.zip) and unzip into a folder on a server that is accessible via network to any users that may want to vote on search results.

The source code is also available [here](http://sxse.googlecode.com/files/sxse-src.zip) in the event that you would like to make any modifications to the application to suit your specific environment.

# Starting the tool #
The Side-by-Side search comparison tool is its own small web server so, after starting it on a server that is accessible to your users, you and your users will be able to access the tool using any standard web browser.

To start the tool, run the following from the directory that contains `sxse.jar`:
```
[JAVA_BINARY_PATH] -classpath ./lib/*:./sxse.jar com.google.enterprise.quality.sxse.Sxse --port=8001 --storage=[STORAGE_DIRECTORY_PATH]
```
e.g., on windows, if you create a subdirectory called storage:
```
java -classpath .\lib\*;.\sxse.jar com.google.enterprise.quality.sxse.Sxse --port=8001 --storage=.\storage
```
There is a `run.bat` file included in the `sxse.zip` file that contains the above command line.


# Logging in #
Once you've started the web server (see **Starting the tool** section above), in any web browser, go to `http://<SERVER_NAME>:8001/login`

e.g., if `SERVER_NAME` is 'eval', go to `http://eval:8001/login`

(Your voters will use this same URL to login and vote for policies.)

The default administrator password is `test`.

Once you login as the administrator for the first time, you should click on the `Password` link on top and change your password.

# Creating Policy Profiles #
Your next step will be to create some Policies.  Policies are what your voters will be comparing.  For instance, Policy A might point to GSA #1 and Policy B might point to GSA #2.

To create policies, click on the Policy Profiles link above.  Then, in the bottom section under `Create a New Scoring Policy Profile`, enter the details of the search engine.

Start by giving it a name.  Make the name descriptive since this will be used later when choosing which profiles you want to compare to each other.  You can set up as many profiles as you like, and then later decide how to mix-and-match profiles for side-by-side experiments.

If you would like to have Google Search Appliance results show up on one or both of the sides of your tool, you'll only need to enter the server name of the GSA server on your corporate network (for instance, `gsa`).  To make sure this works, test entering `http://<GSA_NAME>` into a browser (for instance, `http://gsa`) and make sure you get a search bar that works.  You can also enter other fields in this `Create New Scoring Policy Profile` section for your GSA including the name of the front end, the name of the collection, or other GET parameters.  Those are optional fields that you can leave blank (which means Side-by-Side will use the default front end, default collection, and default GET parameters).

If you would like some other search engine's results to show up on one or both of the sides of your tool (for instance a Google Site Search engine), you just need to enter the URL of how you provide search results minus the very last part which has the actual query term.  For instance, `http://my_search_engine/search?param1=x&param2=y&q=`.  The tool will automatically append the query to the end of the URL string.  This allows for the ultimate flexibility in comparing search results.

Once you've created at least two policies, choose a Policy A and a Policy B using the above radio buttons and click `Update Preferences`.

The other options on this screen should be self-explanatory.

# Uploading Query Sets #
To allow users to rate more easily (which generally provides a lot more voting data for you), be sure to import some query sets that you would like them to test.  This is not necessary (users can enter their own queries) but helps.  And even if you upload query sets, users can still enter their own query at any time using the Side-by-side tool.

A Query Set is simply a flat text file where each line has a query.  The query will be provided to the tool exactly as it appears.

There is a sample `query_set.bat` file included in the `sxse.zip` file that contains some sample queries.


The other options on this screen should be self-explanatory.

# User management #
By clicking on the Users tab above, you can find out information on each user such as when they made their first judgment, their last judgment, and what judgments they made for each query on which they voted.  In fact, if you decided to 'store the results for each judgment' back when you set up the profiles, you'll even be able to see what the results were when they voted on any given query!


# Analyzing the results #
Once your users have voted on enough queries (we'll describe what 'enough' means shortly), you will be able to see which one won.

The tool can report results both as a simple display of the percentages of the judgments for policy A, policy B and equal, and as the probability that A (or B) is better.  As this is a statistical test in the sense of being a finite sample used to estimate characteristics of an underlying distribution, it includes a range of values for the probabilities at various confidence levels.  For example, it will output something like:

<pre>
Judgments for policy_1: 7 of 26 (approximately 26.9% of total)<br>
Judgments for policy_2: 2 of 26 (approximately 7.7% of total)<br>
Judgments for Equal: 17 of 26 (approximately 65.4% of total)<br>
<br>
At 68.0% confidence:<br>
policy_1 P = 0.171 ... 0.367<br>
policy_2 P = 0.000 ... 0.175<br>
<br>
At 90.0% confidence:<br>
policy_1 P = 0.108 ... 0.431<br>
policy_2 P = 0.000 ... 0.238<br>
</pre>

Thus this means that with 90% confidence, you could say that if you always chose policy\_1, you would be making the better choice between 10.8% and 43.1% of the time.  Or with 90% confidence, you could say that if you always chose policy\_2, I would be making the better choice between 0% and 23.8% of the time.

If you look at this, you can see the probability ranges overlap.  This is because our data is too sparse.  In an ideal world, you collect enough data that the ranges no longer overlap, giving you a clear criterion for using the indicating policy.

# Clearing the data #
  * If you want to delete everything, delete the `storage` directory.
  * If you want to delete the judgments, delete the `users` subdirectory and the `results` file.
  * If you want to delete the query sets, delete the `queries` subdirectory.
  * If you want to delete all the preferences (which check-boxes were checked, etc.), delete the `prefs` and the `resultPrefs` files.