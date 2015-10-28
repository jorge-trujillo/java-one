# Continuous Delivery Antipatterns

* Twitter handle: @ags313
* TL; DR; -> Release *more often*!

> Our highest priority is to satisfy the customer through early and continuous delivery of valuable software. - Agile Manifesto

## Issues

* Most issues are more people and organizational challenges
* Delivery is organization specific - in some cases, cannot easily make changes, deploy, etc.
* Suggested book: **Fearless Change: Patterns for Introducing New Ideas** by Mary Lynn Manns

## When a system breaks

* There is some financial impact
* Policies are signs of past failures
  * For example, no deploys on Friday
  * Attempting to control unpleasant outcomes and reduce risk
* Fear of releasing boils down to fear of things breaking
  * Need to build trust
* Signs of fear:
  * Quiet periods, release periods, rc/beta/gold

## Recommendations

* Version your code, use semantic versioning
* Separate master/develop branches (git flow)
* Speed up CI/CD pipeline by parallelizing or reusing intermediate products
* Use a binary repository like Artifactory
* Break and integrate, avoid having separate "release teams" or special teams
  * Team games
* Automate everything you can and that makes sense
  * Test infrastructure-as-code changes before propagating to prod
* Update your runtimes (Java)

## Internet

* Be careful with using maven.org and other sites, they go down.
 * Try running the build without Internet, there may be some interesting things.
 * Apply the same standards to both the app and the environment
* Invest in a binary package repo inside the intranet
