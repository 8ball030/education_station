---
date: 07-24-24
id: 7
modified_at: 07/24/24 13:31
path: ''
tags: []
title: Dev Rewards Analysis Teaser
type: note
---

# OLAS Registry Data Analytics

## Introduction

The OLAS ecosystem is a vibrant network of components, agents, and services. As part of an upcoming initiative, I'm diving deep into the registry data to uncover insights about component ownership, usage, and potential royalty streams. This notebook serves as a peak into the upcoming capabilities of Donation Station, and showcases some initial findings and visualisations.

## Data Description

I'm analysing minted component data from the [OLAS Registry](https://registry.olas.network/).

The main purpose of the analysis is to understand: 
- Distribution of minted components across different categories
- Popularity of components
- Reward mechanisms and top-ups
- Component ownership patterns

This analysis will help incentivise developers by demonstrating the value flow of their components within the OLAS ecosystem.

## Data Loading and Exploration

First, lets discuss what data we have available to us.

There are three main contracts that contain information about the components that have been minted on the registry. Each component has a unique identifier and metadata associated with it.

The three contracts are:

- Components: Protocols, Connections, Contracts, Skills and Custom Components. 
- Agents: Composed of components and configured for specific tasks.
- Services: Composed of multiple agents for providing specific services.


I have implemented a custom command line tool to enable me to both collect and aggregate this data to draw insights about the ecosystem.

We have been able to collect;

A) The code from each component.
B) Ownership data regards to each individual component
C) Reward flows to each Component and Agent.

The custom CLI tool fetches metadata from each registry and downloads code from IPFS for each NFT. 

![](/images/image-9.png)

(I will be releasing the code for this tool soon, so stay tuned for that.)

We have a total of 224 onchain components, however due to minting issues on mainnet, a number of components are not correctly minted meaning their code cannot be retrieved.

![](/images/image-10.png)


## Component Analysis
We have a total of 205 component we are able to retieve the code for.

The authorship of these components (At least according to the data within the component.yaml) is distrbuted as so;

![](/images/image-11.png)

If we break down the components by type, we see the distribution as so;

![](/images/image-12.png)

From my perspective, connections and customs are severly under-represented whilst contracts are over-represented.

Lets breakdown by author and component type;

![](/images/image-13.png)

By removing the outliers, we can get a better view of the average;

![](/images/image-14.png)

We can also see the contributions by the community and changes in this distribution will offer a lot of insight moving forwards.

## Reward Analysis

Lets checkout some of the data about claimable rewards.

We will check the rewards available to each component and sort by the most profitable components.;

![](/images/image-16.png)

We can see that the very very early components are obviously the most profitable as they are used in almost every service down the line.


We can also check what agents are getting the most rewards;

![](/images/image-17.png)


This is all well and good, but what does this look like in terms of real world value? 

Lets check it out.

Assuming a rought price of 1.35 USD per OLAS we are looking at rewards distributed between the components at;

The total value of the rewards for the agents is $46,217.62

The total value of the rewards for the components is $138,652.85

The combined total value of the rewards is $184,870.47


So once we checkout the owner addresses for the NFTS, we can see that the distribution of owners to component rewards is as so;

![](/images/image-18.png)

This allows us to calculate the percentage of rewards to the major contributors.

![](/images/image-22.png)

My personal favourite fact about the data is that of the components, 80 are skills, of those 80 skills, 48 are composable FSMs.

This means that the posibility of composing completely novel applications just from configuring existing behaviours is likely already here.

This could lead to a cambrian explosion in onchain minted components as developers realise how quickly they can compose new applications and build on the shoulders of the giants who came before.



## Conclusion

![](/images/image-23.png)


There is a lot more space in the ecosystem for people to make a name for themselves.

At present, 80% of rewards are flowing to 7 wallets.

Personally, I want to see the distribution curve move a lot more, and to this end I want to help educate developers about the cold hard numbers of what the dev rewards could mean for your decision to enter the space. 

We are talking life changing sums of money at present.

From my perspective, this is a greenfield market with a very few players.

If you want to talk about early as a dev, you really cannot get much earlier than this.

If you want to talk about ecosystem growth, the number of new authors recently entering the space really fills me with excitement for the future, and it has never been easier to start building with the stack.

`8baller`