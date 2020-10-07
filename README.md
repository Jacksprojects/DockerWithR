# Why is R not used in production?

From my personal experience, R has been a fantastic *(and free!)*, flexible and easy to use tool for analytics that benefits from a massive open-source community and doesn't require setting up pesky virtual environments. So, as a user, it's surprising to hear how few companies actually use it in production.

One of the greatest strengths of python, is that it can act like software glue, enabling you to connect it to different languages with ease. As R is not a general purpose programming language, we need to adapt the format of our code to enable it to connect to other devices and software, luckily this is easy to do with the **plumber** package.

## Evolution of making things with R

When I started learning R, the progression of what I was able to make, roughly followed the steps below:

1.  Manually running **R Scripts** with data from local `csv` files to get an output, and running commands in the console for data visualization.
2.  Integrating analysis and reporting with **R Markdown** in the form of presentations, HTML pages, responsive web apps and dashboards using **static content** from a range of data sources such as online databases, web scraping and json files.
3.  Using **Shiny Apps** to **automate** reporting and analysis as a persistent web application hosted on shinyapps.io

This is a great start, but after learning how to create and evaluate data models... how can you actually put them into production? Obviously, running scripts locally and retyping the `predict()` function every time you want an answer is not a sustainable choice, and although Shiny applications are great, they're not the best solution to run one persistent model in a production environment. So what are our options?

A great solution, would be to making an **R API** using the `plumber` package that can interface with other devices or software hosted on google cloud or AWS using a VPS, or even better, as a container on **Docker**.

# First step: Enter, The API

API stands for Application Program Interface, and although it sounds very complicated, you can imagine them as a kind of function. You send off an input such as "What would the output be if the input is X" and the API would give you the answer "Y" in the format of a JSON file. The great thing about APIs is that they can be used to connect a whole range of different programs and languages. This will effectively make your R model cross-platform!

## How do we make and API? It sounds complicated...

It's actually really simple, luckily, the `plumber` package takes care of all of the hard work for us, so literally all we have to do is write the R scripts:

-   The R script that either contains a model or builds the model **(The model script)**

-   The R script to source the model script and tell the API what to give back to you **(The controller script)**

-   The R script which loads to `plumber` package and tells the API which port to listen on and sources the controller script **(The main script)**

A great example can be found on [this](<https://github.com/nolis-llc/r-api-tutorial>) github repo from [Jacqueline Nolis](https://medium.com/@skyetetra)

## Setting up environments is boring

Like with everything else we're trying to automate, why not automate the creation of virtual environments? Virtual private servers are fantastic, but they take a while to set up and it can be a hassle trying to keep all of the versions of software stable and updated.

Enter **Docker**. For those of you who have never heard of it, docker is a technology that allows you to set up light-weight virtual machines called **containers**. For the purposes of this tutorial all you need to know about containers, is that whatever you can do with a VPS, you can do with a container.

Docker works by taking a simple list of instructions that you give it, to automate the creation of a container. That's pretty much it. You have a list of instructions that contain things like, which operating system you want it to have, what software you want installed on it and it creates a container.

The information needed to set up the container can be saved as an **image** that you can then share with other people so that you and whoever else you want to work with can have **identical containers with identical environments**.

This is amazing, as there is an entire community surrounding docker that allows you to download **images** of other containers that someone else has made, so you can just download someone else's image, use it build a new container, then run it on your own computer or on the cloud using 3 lines of code.

We'll use an image of someone else's container to create our own, that runs linux and R version 3.5.0. Then we can write some regular old R scripts to make an API using **plumber** and actually host it using the container!

Cool. But how?
