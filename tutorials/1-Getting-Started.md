# Getting Started

To begin this lab, you should already have Node.js and the Vue CLI installed. If
you have not already done that, see [Learning the Vue
CLI](https://github.com/BYU-CS-260/learning-vue-cli).

Assuming you have that done, you can create a new Vue CLI project:

```
vue create grocery-store
```

Be sure to select **Manually select features** and choose **Router**. For all other
choices you can use the default.

This will create everything needed for this project in a directory called `grocery-store`.
If you are in your GitHub classroom repo, this will look like:

```
lab-first-last/
  grocery-store/
    README.md
    node_modules/
    package.json
    ...
```

If you would like, you can move all those files into the top part of your lab directory:

```
mv grocery-store/* .
mv grocery-store/.[a-z]* .
rmdir grocery-store
```

You can now run:

```
git add .
git commit -a
git push
```

To commit all your code to this repo and push it to GitHub.

Run

```
npm run serve
```

so that you can have a web server running your code. If you visit `localhost:8080`
you should see the default Vue application:

![default Vue Application](/screenshots/vue-default-app-home.png)

## Next Tutorial

[Setup and Navigation](/tutorials/2-Setup-and-Navigation.md)
