# This is an overview of the Presentation and how we want it to flow

This is an idea generation exercise in how I want the presentation to flow. The goal with this presentation is to break it into thirds.

## One Third - Slide Deck Presentation

* Welcoming and Introduction.
  * Welcome to Who put the Sec in DevSecOps(i-ops i-ops)
  * This is who I am
  * This is where I work and how long I have worked with Progress Chef
  * About this presentation. It is going to be both high level and also a bit broad in places. Bare with me and please don't hesitate to stop and ask questions.
  * I'll mention it again, but I'm happy to deep dive later today or any time after if any of this resonates with you!
* Q & A Slide - We're going to start with Q & A. I think it will be funny having the last slide second.
  * Who would consider themselves as being on the DevOps Spectrum (meaning doing some for of Operational tasks for an Application or Platform and use Automation to support that, or could be Developers that have to maintain some aspects of the the platform?)
  * How many people are familiar with Chef or other Configuration as Code / IaC tools.
  * How many people do testing of not just their Application code, but the Platform configuration.
  * Who has heard of the Compliance as Code?
* Background on Progress Chef (#Yes, That Chef)
  * What is Chef Infra - Traditional Infrastructure as Code / Configuration as Code product.
  * What is Chef Inspec - Compliance as Code - taking your System Compute Policies and Standards (CIS / STIG benchmarks) and Codifying them in a human readable (Auditor understandable) method.
  * Open Source discussion. Let's mention Chef as a Commercial Product and CINC as the (free) Community Build of that product.
  * Other Chef products - Habitat as Application Delivery and Courier as Job Orchestration (Coming soon!)
* Scope
  * Understanding that the primary function of Chef - at least initially is Platform Configuration and Persistence.
  * We are going to look at the use cases where we want to codify the deployment of an application to a server in these examples.
* Deeper Dive in Inspec (presentation style)
  * Let's look Compliance as Code and what it is and does.
  * We can validate the compliances (little C) of a server to make sure it complies to your companies compute policies.
  * These can also be Big C Compliance where it applies to items that are involved in an actual audit.
  * `Test-Kitchen` is a tool that we use to apply that application code with the configuration code and tested against that Compliance Profile. We'll talk more about that in a bit.
* Let's Discuss a CIS benchmark for an OS.
  * Walk through some of the CIS Benchmarks and make sure folks understand what that means.
* Demo - Are we ready to go?
  * Let's ask and make sure that folks understand that the platform standard has been defined.

## Two Thirds - Let's draw the DevOps Infinity Loop (Lifecycle)

* DevOps Infinity Loop Picture
  * Walk through the different phases and talk about what they mean (to me)
* We're going to break this into threeISH phases.

### Phase Zero - Plan! This is where you look at your "Human Readable" Compliance Code / Rules

* Security / Platform Team has given us the standards. We should read them! At the very least search for things that might impact us. (thinking back to the CUPS example - search for the packages we might need as dependencies.)
* This could also be the Java Package must be greater than  17. I actually run a no-longer supported application that uses Java 8.
* Looking at this code, knowing it is going to be applied to our platform delivery, we can either pivot up front, talk to security about an exception, or have a deeper discussion with the vendor. We're not going to get into Waivers at this time, just understand they are a thing that allows us to *not fail* a test.

### Phase One - Configure (Code, Build, Test) (We'll get back to Planning as we circle back around)

* Here is where we combine our application & configuration code for our particular need. We Code it.
* This should be a process that is done "locally", meaning on your laptop.
* The Build in our case is utilizing the `test-kitchen` tool to provision a system (vm, ec2, azure, docker), apply the application configuration that we wrote.
* The Test is another function of `test-kitchen` where we can apply those baseline profiles to make sure we didn't fail any. Maybe we missed reading something or just didn't care to read the Compliance Code standards.
* Another thing about Compliance is Code is that *YOU also have configuration standards for your application*, or at least you should. You should be writing your own tests to make sure that the outcome of what you want is there.
  
### Phase Two - Deploy (Test, Release, Deploy) - aka CI/CD

* This phase begins our "hands off keyboard" zone. Many of you may be familiar with "Separation of Duties", that Ops duty from here on out should be automated!
* We usually start this phase automatically when code is pushed to a MAIN branch of a Repo. This is the CI/CD Pipeline!
* Test should overlap with Phase One. We should be doing the exact same test we did locally.
  * This is one of the real "Zero Trust" steps. I don't like throwing that around much but honestly, this whole presentation is really about Zero Trust.
  * This phase is mostly for Auditors and folks that just want to make sure your did your job.
* Release might be the last part that a human touches. Some organizations like to have a final review step, validating that the code is good. Making sure your did what you were supposed to do one more time. A button is hit, and then..
* Deploy. In our case, this is where the cookbook that installs, manages that application is promoted and made available for the servers.

### Phase Three - Manage (Operate, Monitor)

* Again, we kind of blend Deploy and Operate here.
* I can't stress enough that we should all really get to a point where Operate / Support is refactoring the configuration code to accomplish our goals.
* Make this a "Human Free" zone. Have a goal of removing SSH / WinRM / RDP from those systems. Humans make mistakes, or will change one config, etc. There are a lot of Big C Compliance requirements around Change Management and controls. Make that all part of your automation. If your automation say "this is how it is configured", that is how it is documented to be configured.
* Monitor being the last step. This is wrapping up our loop. Those same Compliance as Code profiles / tests can be reported in Chef Automate which is our visibility reporting tool.
* For instance, all of those CIS benchmarks, there may not be Configuration Code to enforce those. If someone logs in and makes a change against the policies, we want to see that reported. A system will show up as Failing Compliance. This can also be tied to alerts and ServiceNow for example to create incidents.
* This Monitoring is also where our internal IT Assessors and Auditors can get their reports to submit as evidence that the system is in the state we say it should be. Selfish plug, the company I used to work for saved 5 million a year be using this tool. (Small sales plug.)

### Rinse and Repeat

* We are then circling back to Plan and iterate as we have additional features, functions, etc. to add.
* We could also get an update of those platform standards / benchmarks and be able to test them again with our code in phase one
* We rerun our pipelines in phase two. You could even orchestrate it so that when the platform standards are updated, your pipeline could re-run and test.
* We can even create tests that we know might fail on some systems in production, just push out the updates to the Inspec Profile, and have them tested in Production and reported in the Monitor step.

### RECAP

> At the end of the day, what we are really doing here is putting the Sec in DevSecOps.

## Three Thirds - The Demonstration

* Let's come back to this but I think we want to do the Apache2 cookbook with Compliance. I need to get to the point where I can push that cookbook in a policy to the Chef Infra server.
* Step One - Local Testing (I'm not going to test with the STIG / OS Standards but we will talk about that.)
* Step Two - Push that cookbook through the pipeline and look at the results. Look at the ci.yml file briefly
* Step Three - Apply that cookbook to the node and look at the compliance results

## Closing Slides

* My LinqApp Walk Through (QR Code for LinqApp)
  * Upcoming Meeting Dates - Including the IaC - Compliance and Platform Standards where we are going to go deeper into this. 
  * Link to Progress Chef - Go to Page and show the Security and Compliance pages.
* Again, if anyone wants to have a deeper discussion on any of this, go to the Linqapp page and click the "Chef / DevOps Consult - Calendar Request