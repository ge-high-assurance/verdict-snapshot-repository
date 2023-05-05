# VERDICT Snapshot Repository

This repository contains snapshot versions of VERDICT Data Model and
since we need these snapshot versions to build other projects and Maven 
central repositories usually contain only release versions of dependencies.

We can use this GitHub project as a Maven repository by putting a
repositories section in a pom.xml like this:

## pom.xml

```xml
<!-- This microrepository has our SADL dependencies -->
<repositories>
    <repository>
        <id>verdict-snapshot-repository</id>
        <url>https://raw.github.com/ge-high-assurance/verdict-snapshot-repository/master/repository</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </repository>
</repositories>
```

I got the idea for using a GitHub project as a Maven repository from
this blog
[article](https://cemerick.com/2010/08/24/hosting-maven-repos-on-github/).

## How to update these VERDICT snapshot dependencies

- Install Java 8 or 11 if needed
- Install Apache Maven if needed
- Check out VERDICT's development branch from GitHub
- Run Maven install in VERDICT/tools

The first build may take up to one hour since Maven may have to
download many poms and jars into its local repository.  Afterwards,
you will be able to run "git pull" to pull any new VERDICT changes from
GitHub, run "mvn clean" to delete any old files, and run "mvn install"
to build new files in much less time than the first build will take.

```bash
# clone VERDICT's GitHub project
git clone git@github.com:ge-high-assurance/VERDICT.git
# or
git clone https://github.com/ge-high-assurance/VERDICT.git
# change to the right directory
cd VERDICT/tools/
# change to the right branch
git checkout development
# delete any old files andninstall new
mvn clean
mvn install --file verdict-back-ends/verdict-bundle/z3-native-libs/pom.xml
mvn package -Dtycho.localArtifacts=ignore
```

- Check out this project from GitHub
- Update the VERDICT snapshot dependencies
- Commit any new changes back to GitHub

These steps will update the snapshot versions of the VERDICT
dependencies in this repository, although any Maven build using
these dependencies will bring in another 10-20 transitive
dependencies as well.  All of these transitive dependencies already
have release versions in the central Maven repositories, so there
is no need to store any of them in this repository too.

```bash
# clone this project
git clone git@github.com:ge-high-assurance/verdict-snapshot-repository.git
# change to this directory
cd verdict-snapshot-repository
# update the VERDICT snapshot dependencies
mvn install
# add any new files
git add .
# commit any new or changed files
git commit -m "Update VERDICT 3.5.0-SNAPSHOT artifacts"
# push any changes back to GitHub
git push
```
