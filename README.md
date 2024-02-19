## Adding the daily DevCard to your GitHub profile


In this article, we will cover two different ways of showing your reading interests through your GitHub profile: 
- manually adding your DevCard by copying the code 
- automatically updating DevCard by using GitHub actions

A while ago, we launched dev cards! A fantastic concept where you can show off how many articles you read, your favorite publications, and show off your rank!

Keen to generate your own dev card?

https://app.daily.dev/devcard

However, this article takes your DevCard to the next level by adding it to your GitHub profile!

For instance, like Ido Shamun

![image](https://github.com/dailydotdev/docs/assets/18360871/9073782a-f0ea-4dd6-a337-31af363152c8)

He added this card to his profile. He created a GitHub action to pull the latest card automatically!

## Creating a GitHub profile page

Perhaps you are still without a GitHub profile and not sure how you can create one yourself.

The process of creating one is not too hard to follow.

Head over to GitHub and create a new public repository.
The name of this repository must be your username.

For example, if your GitHub username is `rebelchris`, the repository name should be `rebelchris`.

![image](https://github.com/dailydotdev/docs/assets/18360871/5dc2ffb2-79df-4900-8f08-6404589fe622)

You’ll even be prompted that you found the secret hack to creating a GitHub profile.


The profile is limited to a markdown file, but don’t worry, there are many cool things you can do to make it look very stylish!

And one of those things is adding the DevCard.

## Manually adding the DevCard to your GitHub profile

On the DevCard page, click `copy code` to copy the code to your clipboard.

![image](https://github.com/dailydotdev/docs/assets/18360871/1a59d98c-1548-46d0-86c6-be429ac62e97)

Head back over to your GitHub profile README.md file and paste the code anywhere in the `README.md` file (you can play around and reposition or resize it at your choice).

If you save this file and view your profile, you should now see the DevCard.

It looks something like this:

![image](https://github.com/dailydotdev/docs/assets/18360871/9073782a-f0ea-4dd6-a337-31af363152c8)

Pretty cool, right. However, there is one downside to this approach.

The DevCard is cached, so it won’t automatically update the content inside it.

However, don’t worry. That’s where the GitHub Action comes in.

## Using a GitHub action to update your DevCard automatically

Ole-Martin decided to make this amazing GitHub action for us!

What it does is it will automatically get your own DevCard and download it to your profile repository.

This means we can automatically run this action every x time and get the latest DevCard.

Let’s see how we can use it for our profile.

Click on the Actions button for your profile repository and set up a new workflow yourself.

![image](https://github.com/dailydotdev/docs/assets/18360871/b3c10113-2510-48ce-a549-d477de48192f)

From there, it will create a basic workflow that we’ll start modifying.

Change the name of the workflow to ‘DevCard’.

Then we want to set two ways our action should be triggered, the first being if there is a push on the master branch.

The other one is a schedule, which acts as a cronjob and will return every {x} time.
In our case, we will run it every night at 00:00.

The workflow_dispatch tells the action it can manually run from the Actions tab.

Then we want to create a new job that will run the DevCard GitHub action.
We need to set one variable for our version, which will be the ID of the DevCard we are fetching.

Making the total file look like this:

```yaml
name: daily-devcard
on:
  workflow_dispatch:
  push:
    branches:
      - main
  schedule:
    - cron: "0 0 * * *"
jobs:
  devcard:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: devcard
        uses: dailydotdev/action-devcard@3.0.0
        with:
          user_id: ${{ secrets.USER_ID }}
          commit_message: "chore: update ${filename}"
```

The next thing we need to do is fetch our USER_ID set it as a secret in this GitHub repository.

Head over to the DevCard page and generate your DevCard.

The ID we need is the part before the .png part.

![image](https://github.com/FrancescoXX/FrancescoXX/assets/18360871/066d05ab-d861-423f-9c04-3d691f42080d)

So in the above example, the URL looks like this:

```
https://api.daily.dev/devcards/QgTYreBqt.png?r=6mk
```

Which means our ID is QgTYreBqt.

Now we can head back to GitHub and click the Settings tab.

From there, choose the Secret section and generate a new secret.

![image](https://github.com/dailydotdev/docs/assets/18360871/8d41b0e9-753e-4627-88b1-43b921b134b3)

This secret should have the following name: USER_ID and the value we just retrieved from the image.

Now we can head over to the Actions tab and run our workflow.
Once the workflow is done, you should see a green icon on your workflow.


![image](https://github.com/dailydotdev/docs/assets/18360871/812c627b-e988-435b-8e56-a410d846e20f)

Head back to your repository, and suddenly you will see there is a new file called devcard.svg.

All we have to do now is update our README.md file to use this generated file like so:


```markdown
<a href="https://app.daily.dev/aaa"><img src="https://github.com/aaa/aaa/blob/master/devcard.svg" width="400" alt="Chris Bongers's Dev Card"/></a>
```

Where you have to modify the href, aaa/aaa, and alt parts to be your repository name.

And there you go! You now have an automatically updating DevCard on your GitHub profile.

## Keep your GitHub action up-to-date with GitHub Dependabot

If you opted for the GitHub action method, you might consider using Dependabot for this repo. It will make sure you are always using the latest version of our GitHub action. ‍ To enable Dependabot for this repository, all you need to do is add a .github/dependabot.yml file with the following contents.

```yaml
version: 2
updates:
  # Maintain dependencies for GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
```

## Conclusion

In this article, we covered two different ways of showing your reading interests through your GitHub profile:

Manual adding your DevCard by copying the code
Automatic updating DevCard by leveraging GitHub actions

I’m sure you can’t wait to generate your DevCard: https://app.daily.dev/devcard.

Don’t forget to share your DevCard on social media using the #DevCard hashtag so we can check out the fantastic profile you created 🤩.
