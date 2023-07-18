# Support Engineer Take Home Assessment (GitLab). 

These are my answers for the take-home assessment for the Senior Support Engineer position at GitLab. I will be responding as a support engineer working with the customer to solve their issues. The format of my response will be in the form of (Q) for the questions and (A) for the answers.


<p style="color:blue;">Q</p> - Write a Ruby or Bash script that will print usernames of all users on a Linux system together with
their home directories. Here's some example output:
gitlab:/home/gitlab
nobody:/nonexistent
.
.
Each line is a concatenation of a username, the colon character (:), and the home directory path
for that username. Your script should output such a line for each user on the system.

Next, write a second script that:
    Takes the full output of your first script and converts it to an MD5 hash.
    On its first run stores the MD5 checksum into the /var/log/current_users file.
    On subsequent runs, if the MD5 checksum changes, the script should add a line in the /var/
    log/user_changes file with the message, DATE TIME changes occurred, replacing DATE
    and TIME with appropriate values, and replaces the old MD5 checksum in /var/log/
    current_users file with the new MD5 checksum.
    Finally, write a crontab entry that runs these scripts hourly.
    Provide both scripts and the crontab entry for the answer to be complete.

A - Hello Customer,

Thanks so much for contacting GitLab Support. My name is Michael Sanchez and I will be assisting you. Is my understanding that you are looking for a scripted way of getting the usernames and their home directories from the /etc/passwd and also taking the full output of the first script and converting it to an MD5 hash. Then on its first run stores the MD5 checksum into the /var/log/current_users file. On subsequent runs, if the MD5 checksum changes, the script should add a line in the /var/log/user_changes file with the message, DATE TIME changes occurred, replacing DATE and TIME with appropriate values, and replaces the old MD5 checksum in /var/log/ current_users file with the new MD5 checksum. Last, it will write a crontab entry that runs these scripts hourly. 

You have required us to provide a Bash or Ruby script to accomplish this task. I will be providing a Bash script that will read the /etc/passwd file line by line. For each line, it uses the cut command to extract the username and home directory, which are the first and sixth fields in the line. It then prints the username and home directory in the desired format.

```bash
#!/bin/bash

while IFS= read -r line
do
    user=$(echo $line | cut -d ':' -f 1)
    dir=$(echo $line | cut -d ':' -f 6)
    echo "$user:$dir"
done < /etc/passwd

```

This is the first script to retrieve the username and home directory information. Now with this script, you can complete the second part of the task.

```bash
#!/bin/bash

first_script=$(bash first_script.sh)

md5_1=$(echo "$first_script" | md5sum | cut -d ' ' -f 1)

if [ ! -f /var/log/current_users ]; then
    echo "$md5_1" > /var/log/current_users
else
    
    md5_2=$(cat /var/log/current_users)
    if [ "$md5_1" != "$md5_2" ]; then
        echo "$(date) changes occurred" >> /var/log/user_changes
        echo "$new_md5" > /var/log/current_users
    fi
fi
```

Crontab
```bash
0 * * * * first_script.sh && /bin/bash second_script.sh

```
Please go ahead and try this script and let us know if it was of any help.

Regards.

Q - A user is complaining that it's taking a long time to load a page on our web application. In your own
words, write down and discuss the possible cause(s) of the slowness. Also, describe how you would
begin to troubleshoot this issue.

Keep the following information about the environment in mind:
    The web application is written in a modern MVC web framework.
    Application data is stored in a relational database.
    All components (web application, web server, database) are running on a single Linux box with
    8GB RAM, 2 CPU cores, and SSD storage with ample free space.
    You have root access to this Linux box.

A - Hello Customer,

Thanks for contacting GitLab Support my name is Michael Sanchez and I will be assisting you. Based on your issue there could be several reasons why the web page is loading slowly. Here are some potential causes.

### 1 - If there are many users accessing the application at the same time, it could be causing high CPU usage or memory consumption, slowing down the application.

### 2 - If the application is making a lot of database queries or the queries are complex, it could be slowing down the page load time. The database might also be poorly indexed, causing slow query execution.

### 3 - If the server is located far from the user or if there's network congestion, it could increase the time it takes for data to travel between the server and the user, causing slow page loads.

### 4 - The application code might have database queries that are taking a long time to execute. This could be due to poor design or lack of optimization.

With that being said in order to troubleshoot the possible cause I suggest you follow this approach.

Check the application logs for any errors or warnings that might indicate problems with the application code or database queries.

```bash
tail -f /var/log/application.log

```

If you are using MySQL or PostgreSQL use a tool like `EXPLAIN` to analyze your database queries. This can help identify slow queries or missing indexes. For example 

```bash
EXPLAIN SELECT * FROM name-of-table WHERE field = 'a-value';

```
Use commands like `top`, `htop`, or `vmstat` to monitor CPU and memory usage. If you see high usage, it could indicate a server load issue. You might need to optimize your application or upgrade your server. For example.

```bash
top
```
Look at the %CPU and %MEM columns to see how much CPU and memory each process is using.

Use commands like `ping` or `traceroute` to check the network latency between the server and the user. If the latency is high, you might need to consider using a CDN or moving your server closer to the users.

Please go ahead and try that and let us know if it was of any help.

Regards.


Q - Study the Git commit graph shown below. What sequence of Git commands could have resulted in
this commit graph?

A - Hello Customer,

Thanks for contacting GitLab support my name is Michael Sanchez and I will be assisting you. The Git commit graph you provided shows two branches, master and feature. The feature branch was created from the master branch, some commits were made on it, and then it was merged back into the master. For your reference, I will list a sequence of git commands that could have resulted in this commit graph.


First, we start with a repository that already has some commits represented by A and B on the master branch.

## Create a new branch called feature.
```bash
git checkout -b feature
```

## Make changes to the code, and commit these changes.
```bash
git add .
git commit -m "First commit"
```

## Make more changes, then commit it.
```bash
git add .
git commit -m "Second commit"
```

## Switch back to the master branch.
```bash
git checkout master
```

## Merge feature into master.
```bash
git merge feature
```

This sequence of commands would result in the commit graph. The actual commit messages and the specific changes made in each commit would depend on what was done in the repository. I hope this is helpful for you, let me know your thoughts.

Regards.


Q - GitLab has hired you to write a Git tutorial for beginners on: Using Git to implement a new
feature/change without affecting the main branch. In your own words, write a tutorial/blog explaining things in a beginner-friendly way. Make sure to address both the "why" and "how" for each Git command you use. Assume the
audience are readers of a well-known blog.

A - Hello Customer,

Thanks for contacting GitLab Support my name is Michael Sanchez and I will be assisting you. You have requested a friendly git tutorial on how to implement a new feature/change without affecting the main branch. I will provide you with guidance and will explain the reasoning behind it. I hope this tutorial will be of any use to you and that you'll find valuable information.

Follow this tutorial:

# Implementing a New Feature with Git Without Affecting the Main Branch

In this tutorial, we're going to talk about how to use Git, a powerful version control system, to implement a new feature or change in your project without affecting the main branch. This is a common scenario in software development, where you want to add a new feature or fix a bug, but you don't want to disrupt the stable version of your code.

## Why Use Git Branches?

In git, the main branch often called master is typically where the stable version of your project lives. It's the code that you know works and is ready to be deployed or shipped at any time.

When you want to add a new feature or make a change, you don't want to do this directly on the main branch. Why? Because changes often involve experimentation and might introduce bugs. You don't want these uncertainties in your stable codebase.

This is where git branches come in. A branch in git is like a parallel universe for your project where you can make changes without affecting the main line of development. Once you're happy with your changes, you can merge them back into the main branch.

## How to Use Git Branches?
Let's walk through the process of creating a new branch, making changes, and merging them back into the main branch.

## Create a New Branch

First, navigate to your project directory. Then, create a new branch using the git checkout -b command followed by the name of your new branch. For example, if you're adding a new feature called "api", you might name your branch feature/api.

```bash
git checkout -b feature/api
```
This command does two things: it creates a new branch called feature/api, and it switches you over to that branch.

## Make Changes and Commit
Now you're in your new branch, and you can start making changes to your code. Go ahead and implement your new feature or change.

Once you've made some progress, you'll want to commit your changes. This is like saving a snapshot of your work. First, you need to stage your changes with git add. If you want to stage all changes, you can use git add .

```bash
git add .
```
Then, commit your staged changes with git commit. You'll need to include a message describing what changes you've made. Make this message clear and descriptive for you and others to understand what was done.

```bash
git commit -m "Implement an API feature"
```
## Switch Back to the Main Branch
Once you're done with your changes and have committed them, you can switch back to the main branch.

```bash
git checkout master
```
## Merge Your Changes into the Main Branch
Now you're back in the main branch, and it's time to bring in the changes from your feature branch.

```bash
git merge feature/api
```
This command merges the feature/api branch into the current branch which is master.

And that's it! You've successfully implemented a new feature in a separate branch and merged it back into the main branch.

### Conclusion
Using branches in git is a powerful way to work on new features or changes without disturbing the main line of development. It allows you to experiment and make mistakes without fear of messing up your stable code. Once you're happy with your changes, you can easily merge them back into the main branch. 

Hope you found your answers in this tutorial.

Regards.

Q - What is a technical book/blog you read recently that you enjoyed? Please include a brief review of
what you especially liked or didn’t like about it.

A - Hello Customer,

Thanks for contacting GitLab Support my name is Michael Sanchez and I will be assisting you. You have asked what is that tech book or blog that I'm currently reading and enjying. And also provide a brief review of what I especially liked about it and what I did not like. To answer your question right now I'm reading a few technical books including *Ansible for Real-Life Automation* by Gineesh Madapparambath and I'm also reading few blogs including Adaltas: https://www.adaltas.com/en/2023/06/01/dev-environment-terraform-lxd/.

But the tech blog I'm enjoying at the moment is Minio on Kubernetes: https://levelup.gitconnected.com/minio-on-kubernetes-71ce34da7a19.
The article "MinIO on Kubernetes" on gitconnected.com is a comprehensive guide that provides a step by step approach to deploying MinIO on Kubernetes. Here are some key points:

*Positives:*

### Detailed Instructions: 
The article provides clear, step by step instructions that are easy to follow. It includes code snippets and command line examples which are very helpful for readers who are implementing the steps.

### Use of Visuals: 
The use of diagrams and screenshots helps readers understand the concepts better and provides a visual confirmation of what they should see on their screens.

### Explanation of Concepts: 
The author does a good job of explaining what MinIO and Kubernetes are, and why someone might want to use them. This is helpful for readers who are new to these technologies.

*Areas for Improvement:*

### Assumed Knowledge: 
The article assumes a certain level of knowledge about Kubernetes and YAML, which might make it difficult for absolute beginners to follow along.

### Lack of Context: 
While the article provides a detailed guide on how to deploy MinIO on Kubernetes, it doesn't provide much context or examples on why and when you would want to do this in a real world application.

### No Discussion on Alternatives: 
The article could have been improved by discussing alternatives to MinIO and why MinIO might be a preferred choice.

Overall, the article is a solid resource for someone looking to deploy MinIO on Kubernetes, especially if they already have some familiarity with these technologies.

Let me know if this was of any help.

Thanks and regards.







