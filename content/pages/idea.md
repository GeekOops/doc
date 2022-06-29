---
title: Idea
description: Presentation of the general idea of geekoops
date: 2022-06-29T13:49:31+02:00
Lastmod: 2022-06-29T13:49:31+02:00

---
geekoops is a collection of generic devops and sysops recipes with a strong focus on the openSUSE distribution. The main idea is to provide tested and simple-to-use building blocks for system administrator and engineers to easily setup and maintain their systems.

I started this project during the [SUSE Hackweek 20](https://hackweek.suse.com/20/projects/create-ansible-roles-for-generic-server-stuff) based on a personal need for some generic and easy-to-use ansible roles. The project was born from the believe that many more people have the same need and building something publicly available might help the one or another person in fulfilling its daily job.

## Simplicity is key

The core idea is to build re-usable and simple deployment recipes that work well together but are loosely coupled. Simplicity is favored over feature completeness, and automated testing heavily encourage to ensure that the project remains healthy over time.

Often some commonly used ansible support many configuration parameters and scenarios but do not work well together with openSUSE. Many moving parts means many corner cases and often those roles do not work well on openSUSE distributions, because either something missing or something is configured just different enough to make the orchestration tools stumble.

This is what geekoops wants to make different: The recipes here have a focus on openSUSE but should remain simple enough, so that they should work on other distributions as well or at least lower the threshold of making them work with other distributions.

Simplicity often is a synonymous for maintainability and a key ingredient to pass the test of time.

in a nutshell:

> geekoops heavily encourages the keep-it-simple-stupid principle