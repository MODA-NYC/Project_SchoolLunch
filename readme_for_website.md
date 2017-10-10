---
permalink: ????
layout: project
title: Free Lunch For All!
lead: We partnered with DOE to find a method to optimize the universal school lunch program, where all students are entitled to free breakfasts and lunches.

excerpt: ????
current: true
image: /img/plot.png
image_accessibility: final groupings
open_datasets:
github_repo: https://github.com/moda-nyc/doe_MealReimbursements.git
---


We partnered with the Department of Education to find a method to optimize the universal school lunch program, where all students are entitled to free breakfasts and lunches.

{% contentfor scoping %}

**Providing free universal lunch to all students**
The federal government offers meal subsidies to schools participating in the National School Lunch Program, run by the USDA. Traditionally this worked by giving a subsidy per meal based on the income level of the individual student's family. Because this leads to stigmatization of students from low income families, there has been a move towards more inclusive programs providing universal lunch to all students regardless of their family's income level. The Community Eligibility Provision (CEP) was created by the federal government in 2010. Under this program schools receive subsidies based on the overall makeup of the school (or group of schools). Participating schools are required to provide breakfast and lunch at no charge to all students. More information on NYC's initial involvement in CEP is available in a [Fiscal Brief](http://www.ibo.nyc.ny.us/iboreports/if-no-student-pays-cost-to-provide-free-lunch-for-all-of-new-york-citys-elementary-school-students.html) by the NYC Independent Budget Office.

**Analytics Question**
The amount of federal reimbursement for CEP depends on the percentage of identified students or students categorically eligible for free meals. This percentage is based on the student’s participation in other government program such as SNAP and TANF. Schools can be enrolled as groups and there are no geographic or other constraints on how to group the schools. The subsidy parameters, and hence the reimbursement amount, change based on how the schools are grouped. The challenge is to find the optimal school groupings to maximize the reimbursement.

{% endcontentfor %}

------------

{% contentfor data %}

For each school the following data is needed:
* the number of identified students,
* the total number of students, 
* the projected number of breakfasts and lunches for the coming year.

The projected number of meals is based on meals served in previous years. Because most of these schools were not part of a universal lunch program prior to this, the meal projections will likely be different from the actual meals served. However because the school groupings need to be submitted before the meals are calculated, an exact number is not possible.

{% endcontentfor %}

------------

{% contentfor analysis %}

This is a combinatorial optimization problem, similar to some very well-known problems such as the traveling salesman and the knapsack problems. These are all are easy to state but hard to solve. 

This problem is straightforward to solve for a handful of schools. The reimbursement can be calculated for each possible grouping and the highest grouping would be chosen. But as the number of schools grows this type of exhaustive or brute force approach quickly becomes impossible. With over 1000 schools an exact result was out of reach, but we were able to find an approximate solution. 

We tried a number of approaches and found Monte Carlo to be the most elegant to implement and also gave the best results. Monte Carlo techniques have traditionally been used as a clever way to simulate complicated probability distributions. In this case, we were only interested in the maximum value (highest reimbursement), so we used two well known derivations: stochastic hill climbing and simulated annealing.

We implemented these approaches in Python, using Pandas vectorized methods to reduce runtime. In addition we used the multiprocessing package to do runs in parallel and choose the best of the lot.
There may be other methods giving as good or better results. Genetic algorithm implementations may also be a good candidate for this type of problem.

{% endcontentfor %}

------------

{% contentfor pilot %}

We initially intended to run this algorithm on NYC schools new to the CEP program for the 2017-2018 school year. However because of an administrative change in how identified students are counted, a single grouping now yields the highest possible reimbursement. 

School districts who could potentially benefit from this approach would have at least 30 schools and a low identified student rate of under 62.5% 

{% endcontentfor %}

------------

{% contentfor handoff %}

DOE will be able to use this algorithm in subsequent years to calculate the school groups.  After the first year, DOE has the option to keep schools in their current group or regroup them for a higher collective reimbursement. The algorithm can be adapted to address this.

{% endcontentfor %}

