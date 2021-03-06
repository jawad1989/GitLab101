# TABLE OF CONTENTS:

1. Getting Accesss Token in Jira
2. Gitlab integration Settings
3. Example Adding Comments
4. Example Changing Status to Closed from Open
5. Gitlab Jira DEvelopement Panel Integration - Silver Gold plans

*************

# 1. Getting Accesss Token in Jira
 * After logging in create your first project
 * Create a issue in Jira
 
 ![Create Issue](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/create-issue.PNG)
 
 * Goto `Settings->Account Settings->Security` and create a token, copy the token 
 
 ![API Token](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/api-token.PNG)
 
 
# 2. Gitlab integration Settings
  * Goto to Gitlab Project. `Settings->integrations->Jira`
  
  ![Jira](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/gitlab-integration.PNG)
  
  1. Add the URL of JIRA 
  2. Add transition Ids
  3. Add the API token obtained from above
  
  ## Obtaining Transitition Ids
  
  1. By using the API, with a request like `https://yourcompany.atlassian.net/rest/api/2/issue/ISSUE-123/transitions` using an issue that is  in the appropriate “open” state
  2. By mousing over the link for the transition you want and looking for the “action” parameter in the URL
    * our transition ids would be `11,21,31`
 
# 3. Example Adding Comments
   1.  Once you have created an issue in JIRA e.g. `TIER-6`
 ![issue created](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/issue-created.PNG)
 
 2.  In your gitlab project create a branch by adding file, in commit message add `TIER-6 #comment my new comment` and in branch add `TIER-6-demo-branch`
 
 ![new-branch](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/add-new-commit.PNG)
 
 you will see the end result says close TIER-6 
 ![close-tier6](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/merge-request.PNG)
 
  3. Once you pressed murge you will see once merge-request in in progress it will add the new comment in jira
 
 ![mergre-requst](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/commit-tier6.PNG)
 
  4. Once merge is complete you will see the status changed 'TIER6 Closed' in gitlab
 ![tatu-closed](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/closed-tier6-gitlab.PNG)
 
  5. You can also see the issue moved to Done(closed)
 ![jira-closed](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/closed-jira.PNG)
 
# 4. Example Changing Status to Closed from Open
pending 

# 5. Gitlab Jira DEvelopement Panel Integration - Silver Gold plans
ONLY AVAILABLE: `GOLD` and `SILVER`

* in this integration you can see the gitlab's merge/branch/commit counts in JIRA issues but its only available in SILVER and GOLD plans
> https://docs.gitlab.com/ee/integration/jira_development_panel.html

Steps to integrate

1. Click your `avatar->settings->Applications`
2. Create a new application
  ![new app](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/create-a-application-2.PNG)
  
  ![app craeed](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/create-a-application.PNG)
  
3. Get the group name or create a new group with some projects in gitlab
![group-gitlab](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/create-a-group.PNG)

4. goto manage dvcs and setup gitlab app's group

link: https://jawadhhsc.atlassian.net/secure/admin/ConfigureDvcsOrganizations!default.jspa

![gitlab-dvcs](https://github.com/jawad1989/GitLab101/blob/master/Jenkins-Integration/images/manage-dvs.PNG)



# Resources:
https://www.youtube.com/watch?v=SwR-g1s1zTo
https://docs.gitlab.com/ee/integration/jira_development_panel.html
https://gitlab.com/gitlab-org/gitlab/blob/8-13-stable-ee/doc/project_services/jira.md
https://docs.gitlab.com/ee/user/project/integrations/jira.html
https://gitlab.com/gitlab-org/gitlab/-/issues/8185
https://www.youtube.com/watch?v=y74brTo5EBA&t=469s
https://jawadhhsc.atlassian.net/login/oauth/callback
https://jawadhhsc.atlassian.net/rest/api/2/issue/TIER-2/transitions
