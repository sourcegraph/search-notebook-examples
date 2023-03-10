# Find log4j dependencies across all your code

This notebook was linked from our in-depth Log4j 0-day fixes and mitigations [blog post](https://about.sourcegraph.com/blog/log4j-log4shell-0-day/).

Run these queries on Sourcegraph to quickly determine which projects directly depend on vulnerable versions of log4j.
The following notebook contains search queries that identify vulnerable dependencies on Sourcegraph Cloud across 2M public repositories.

Although code search is a fast and versatile tool for assessing the impact of a novel vulnerability, it's not perfect. These build 
systems have no convention for dependency lockfiles, so the queries below won't find projects where log4j is a transitive or an indirect dependency (because 
there's no file committed to Git that lists the fully resolved dependencies and versions).

## Gradle

```sourcegraph
org\.apache\.logging\.log4j' 2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)(\.[0-9]+)) 
lang:gradle patterntype:regexp count:all
```

## Maven

```sourcegraph
<log4j\.version>2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)(\.[0-9]+))</log4j\.version> 
file:pom\.xml patterntype:regexp 
```

## Ivy

```sourcegraph
org="org\.apache\.logging\.log4j".*rev="2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)(\.[0-9]+))" 
file:ivy\.xml patterntype:regexp count:all
```

## SBT (Scala)

```sourcegraph
"org.apache.logging.log4j" % "2.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)(\.[0-9]+))
file:\.sbt$ patterntype:regexp count:all
```

## Bazel

```sourcegraph
org\.apache\.logging\.log4j: 2.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)(\.[0-9]+))
lang:bazel patterntype:regexp count:all
```

## Any file containing `org.apache.logging.log4j` followed by a vulnerable version number

```sourcegraph
org\.apache\.logging\.log4j 2.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15)(\.[0-9]+))
patterntype:regexp count:all
```

https://sourcegraph.com/github.com/SonarSource/sonarqube@601e7fbb0ca7cd323b69742e15cd016dac46cf62/-/blob/build.gradle?L367

## Example: a list of files to update

https://sourcegraph.com/github.com/projectlombok/lombok@37d548c7fa1db5284227a861089857fc20de06ce/-/blob/buildScripts/ivy.xml?L48

https://sourcegraph.com/github.com/lagom/lagom@12e8ee61f0f4c3a3658fd569b5562c0de2b292c5/-/blob/docs/manual/java/guide/logging/code/build-log-lang.sbt?L27
