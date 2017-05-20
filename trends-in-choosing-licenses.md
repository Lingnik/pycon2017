# The trends in choosing licenses in Python ecosystem
* presenter: Anwesha Das @anweshasrkr
* IANAL/IAAL - Not legal advice.
* Lawyer/blogger translating legalese to English.

# Three components of a good community
* Good contributor base
* Just administration
* Strong licensing model

# What is a license?
* Driving License- Permission to drive a vehicle on the road
* Software License- Permission to do certain things with the software

# Who gives you the license?
* The owner of the software.
* Owner gets permission from a legal regime--copyright.

# What is copyright?
* "Right over a copy."
* Exclusive right to use, distribute, sell the original creation, granted for a limited period of time.

# Why is software copyrighted?
* Dev solves a problem through code -- through choice or expression.
* That makes us "code poets" or "artists".
* We don't need to apply for the copyright.

# PyPi- Python Package Index
* 100k packages

# Anwesha's Project- The license usage in Python ecosystem
* Which licenses are favored by developers?
* How many packages are there without licenses mentioned?
* How do you correct those licenses?
* What is the licensing scenario of the Python world?

# Process
* Did license analysis on top 2k projects in pypi.
* How? PyPI Ranking site.
* Got the names of the top 2k projects.
* Queried PyPI JSON API for "license" keypair.
* Charted- bubble, pie

# Results
* BSD is the top, 28% of usage
* MIT follows, 25% usage
* GPL & Apache tied @ 12%

# Many "Unknown"s
* Many not licensed- show up as Unknown or noname.

# Categories of Licenses

## Proprietary Licenses
* Limited, restricted rights granted to the user.
* User does not have any access to the source code.
* All rights issued in End-User License Agreement, EULA.
* Generally issued for a specific period of time.
* User accepts either in writing or, more commonly, in-product.

## Public Domain
* Intellectual property rights are relinquished.
* Ways a work can be subject to public domain.
    * Write it as "public domain"
    * Choose Creative Commons 0.
        * Why? In some cases, it's not possible to put something in public domain.

## Open Source
* 10 preconditions fixed by the open source initiative.
* Imposition of duties, we can split them into several categories...

### Permissive License
* Non-limiting, compatible license.
* Serious disadvantage: it can be used in proprietary applications.
* Affects the final user's future freedom.
* Two categories..*[]:

#### Highly Permissive Licenses - MIT/BSD
* "It's MIT or BSD for me."
* One condition only: must retain the credits status, the name, and the copyright declaration intact.
* Carries a high risk of proprietary usage.
* Similar to ex pat licenses.
* Origin of MIT License is Massachusetts Institute of Technology
* Origin of BSD License is Berkeley
* Modification is permitted
* Private license is permitted
* Sublicensing is permitted

#### Lesser Permissive License - Apache
* Unique clause: Grant of Patent License
* If a company has a paten on a particular software, reuses a open source product that implements that algorithm
* If issued under a non-grant of patent license, then difficult
* If issued under grant of patent license, then company saying "we won't sue you related patents on software we give you"

### Reciprocal Licenses
* User has to perform some duties in return.
* e.g. GNU General Public License
* GPL says you should pass on the full freedoms you got with the software so the software spreads throughout society.
* Four freedoms:
    1. The freedom to run the program as you wish, for any purpose.
    2. Ensures the freedom to modify the source code.
    3. The freedom to redistribute copies so you can help your neighbor.
    4. The freedom to distribute copies of your modified versions to others.

### Copyleft
* A masterplan or policy for the promotion/advancement of free software.
* Most prominent plan to spread this software freedom.
* Clever hack on copyright laws.
* Permissive vs Copyleft- Permissive license can be used in proprietary SW, but Copyleft cannot.

# Implementation Guidelines
* Choose a proper license based on use-case
    * Choosing a license creates a boundary for yourself
    * The license shows the aim/intention of the software
* Include LICENSE file in your source code. (or LICENSE.md or LICENSE.txt)
    * Should include name of the license
    * Should include the full text of the license document itself
* Add a copyright header to each significant source code file
    * Modify it from time to time as you make new releases
* Mention the license in setup.py and in the classifier
* Mention the license name in REAMDE file & direct user to LICENSE file
* When freedom is important, GNU license is for you.
* **Do not invent your own license.**
    * If you make the bad decision to do so, don't be funny.

# Which license to choose?
* Plenty of websites to solve this problem.
    * Open Source Initiative
    * copyleft.org
    * Fedora Project
    * choosealicense.com
* Choose a popular license (in your community).
    * You're also choosing a community--the constitution of that community.

# While Choosing Your License
* Read your license document carefully before you choose one.
    * There are licenses that require you to do certain things.
    * Do them, or you won't get the license protection.
    * e.g. MIT License--many marked themselves as licensed under MIT,
      but they don't mention the full license text in any substantial
      portion of their license. MIT License requires them to do that.
* Spend some time while choosing a license.
