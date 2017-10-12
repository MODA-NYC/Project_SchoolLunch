---
permalink: ????
layout: project
title: Free Lunch For All!
lead: MODA partnered with DOE to find a method to optimize the universal school lunch program, where all students are entitled to free breakfasts and lunches.

excerpt: ????
current: true
image: /img/plot.png
image_accessibility: final groupings
open_datasets:
github_repo: https://github.com/moda-nyc/Project_SchoolLunch.git
---
**Providing free universal lunch to all students**
The federal government offers meal subsidies to schools participating in the National School Lunch Program, run by the USDA. Traditionally this worked by giving a subsidy per meal based on the income level of the individual student's family. Because this leads to stigmatization of students from low income families, there has been a move towards more inclusive programs providing universal lunch to all students regardless of their family's income level. The Community Eligibility Provision (CEP) was created by the federal government in 2010. Under this program schools receive subsidies based on the overall makeup of the school (or group of schools). Participating schools are required to provide breakfast and lunch at no charge to all students. More information on NYC's initial involvement in CEP is available in a [Fiscal Brief](http://www.ibo.nyc.ny.us/iboreports/if-no-student-pays-cost-to-provide-free-lunch-for-all-of-new-york-citys-elementary-school-students.html) by the NYC Independent Budget Office.

We partnered with the Department of Education to find a method to optimize the universal school lunch program, where all students are entitled to free breakfasts and lunches.

{% contentfor scoping %}

The amount of federal reimbursement for CEP depends on the percentage of identified students or students categorically eligible for free meals. This percentage is based on the student’s participation in other government program such as SNAP and TANF. Schools can be enrolled as groups and there are no geographic or other constraints on how to group the schools. The subsidy parameters, and hence the reimbursement amount, change based on how the schools are grouped. The analytics question is how to find the optimal school groupings to maximize reimbursement.

**Reimbursement Rules**

Schools are reimbursed at either the "free" rate or the "paid" rate for each meal. (Traditionally there is also a "reduced" rate, but that does not apply here). The fraction of meals which are reimbursed at the "free" (this is the higher of the two rates) is determined by the group the school is enrolled under. This fraction we will call the $threshold$. The rest of the meals get reimbursed at the "paid" rate.

At a minimum, the paid rate, $r_{lunch/breakfast,paid}$, for all meals is subsidized. This includes total number of breakfasts for all schools $\sum_{s}B_s$, and total number of lunches for all schools $\sum_{s}L_s$. That means the base reimbursement, $R_{base}$, is independent of the groupings: 

$\begin{align}
R_{base} = r_{lunch,paid} \sum_{s}L_s + r_{breakfast,paid} \sum_{s}B_s
\end{align}$

So then total Reimbursement, $R$, is

$\begin{align}
R = \sum_{s} threshold_s (\Delta r_{lunch} L_s  + \Delta r_{breakfast} B_s) + R_{base}
\end{align}$

> where 
> $\begin{align}
&\Delta r_{breakfast} = r_{breakfast,free} - r_{breakfast,paid} \\
&\Delta r_{lunch} = r_{lunch,free} - r_{lunch,paid}
\end{align}$

> and the $threshold_s$ for each school, $s$, is determined it's group, $g$. It is the fraction of "identified students" $I_s$ for all schools in the group $g$ to the total number of "enrolled students" $N_s$ for all schools in the group, multiplied by a constant. This constant is set by the USDA at 1.6 but may change in subsequent years. $I_s$ and $N_s$ are based on enrollment in the previous school year. "Identified students" are those deemed categorically eligible for free meals, primarily due to receiving some sort of public assistance benefit.

> $\begin{align}
threshold_s =Min(1, \frac{ \sum_{s\in g} I_s} {\sum_{s\in g} N_s } * 1.6 )
\end{align}$

Before the school year begins, the groups are set and reported to the state. How schools should be grouped together is not prescribed. 

Reimbursements happen on a monthly basis after the school year begins and are determined the actual number of breakfasts and lunches eaten by the students. The groupings however, can only be based on projected meal counts.


**Grouping Rules**
* All groups have to meet the minimum threshold: $threshold_{min} = 40\% * 1.6 = 64\%$
* There can be at most 9(?) groups
* All schools must be part of the program

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

This is a combinatorial optimization problem, similar to some very well-known problems such as the traveling salesman and the knapsack problems. The challenge is finding an optimal solution among a descrete set of objects.

This problem is straightforward to solve for a handful of schools. The reimbursement can be calculated for each possible grouping and the highest grouping would be chosen. But as the number of schools grows this type of exhaustive or brute force approach quickly becomes impossible. With over 1000 schools an exact result is well out of reach, but we were able to find an approximate solution. 

We tried a number of approaches and found Monte Carlo to be the most elegant to implement and also gave the best results. Monte Carlo techniques have traditionally been used as a clever way to simulate complicated probability distributions. In this case, we were only interested in the maximum value (highest reimbursement), so we used two well-known derivations: stochastic hill climbing and simulated annealing.

We implemented these approaches in Python, using Pandas vectorized methods to reduce runtime. In addition we used the multiprocessing package to do runs in parallel and choose the best of the lot.

The details of our approach and analysis can be found [here](https://github.com/MODA-NYC/Project_SchoolLunch/blob/master/FreeLunch-MonteCarlo.ipynb), in our github repo.

{% endcontentfor %}

------------

{% contentfor pilot %}

We initially intended to run this algorithm on NYC schools new to the CEP program for the 2017-2018 school year. However because of an administrative change in how identified students are counted, a single grouping now yields the highest possible reimbursement. 

School districts that could potentially benefit from this approach would have at least 30 schools and a identified student rate of under 62.5% 

{% endcontentfor %}

------------

{% contentfor handoff %}

DOE will be able to use this algorithm in subsequent years to calculate the CEP school groups. This will be useful if the percentage of identified students as a whole falls below 62.5%, currently it is very close to that limit and is expected to decrease. After the first year in the program, the school can stay with their previous threshold or be regrouped them for a higher collective reimbursement. The algorithm can be adapted to address this.

{% endcontentfor %}


