# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Alexander Michelberger

```
Please write your explanation here...

1)
The first instance of sampling in the procedure is on line 47. The command is np.random.choice. Here we are using the people index, essentially each of the 1000 people can be identified by 1 number. From this list of index values, 10% are randomly selected (not clustered, stratified or systematic) as being infected. This is denoted with attack rate = 0.10 times the length of the list of the ppl dataframe. It is important to mention, there is no replacement, understanding, once somebody is infected they should not be re-considered for infection again, and to match the article assumption that everyone has a 10% chance of being infected. This will create a sample size of 100 infected people. Here the sampling frame are those in the community that have attended 1 of the 2 weddings or 80 brunches within the specified time frame. With regards to the blog post, this refers to the line under the table. It is the rephrase of 10% sampled per event to 10% probability of the entire community. THis is why in the code, ATTACK_RATE is multiplied to the entire dataframe, rather than a preset stratification by a group.  

2) The second instance of sampling is on line 51. The command is np.random.rand and is used to mark the primary contact tracing. Here, the infected participants are selected for the sample. Although we still have access to the population of the community, the sampling frame is all people that were infected, since we only chose infected individuals with the code. This relates to the text under the second graph. It is mentioned infection tracing is only 20% of finding the source event, hence we have TRACE_SUCCESS set to 0.20. If we were to look into the data, about 20% of the infected individuals will have traced = True. The rest of the infected individuals, it will say False. Individuals not infected will have null. The number of True for traced should be about 20%, but can very, similar to the distribution of the first graph, which shows proportions of weddings ranging around 0.20. 

3) The last instance of sampling is on line 55. This comes from the event_trace_counts >= SECONDARY_TRACE_THRESHOLD. This is the secondary contact tracing, where if two people were infected from the same event, then everyone at the event is tested and will identify 100% of the infections at that event. This is marked by SECONDARY_TRACE_THRESHOLD as 2. All that is needed are two confirmed cases from the same event. Here the sampling frame is already those infected, and traced. We had to reduce even smaller because we are restricting to those tested. This means the sampling frame is now restricted to the 100 people infected. 

The distributions of the data are binomial thanks to the True/False spliting of the data. 


**Does the file reproduce the same graphs from the article by Whitby?**
No, the script does not reproduce the graphs from the Whitby article. 
This difference in graphs was expected because of the sampling procedures. Since the commands used are inherently random, the data output we get will lead to different graphs. Although the messaging may be the same, we do see differences in the graphs. 


**Comment on the reproducibility of the results.**
When changing the simulation event to 1000 times and re-running multiple times, the graphs were not identical.  
THe graphs did come out quite similar, in which the proportion of cases varied around 0.20 with changing frequency values. The bars representing infections from weddings and traced to weddings follow similar distribution patterns.  
Again, this is expected, because we have the sampling codes input into the scripts we are running. This will change the makeup of the data every time, and thus change the visualization everytime the script is run. 


**Describe the changes you made to the code and how they affected the reproducibility of the script file**
To ensure the data is reproducible, I added np.random.seed(42) into the function. This ensures the random functions, which we use for sampling, generate the same samples and thus distributions of data.   
This will ensure, regardless of who or when the code is executed, the same data is generated over and over again. This allows us to reproduce the code for the purposes of comparison and reproducibility.
```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
