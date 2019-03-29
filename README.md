# sbt-alldocs

Put all the documentation for a project in one place. Suitable for distribution of an alldocs for an
alljar (aka uberjar). This will include all compiler version targets as well. Resulting in a large,
but complete, set of documentation.

## Usage

This plugin requires sbt 1.0.0+

This adds a command `allDocs` which:

1. Collects all documentation artifacts for dependencies. This is all dependencies minus those in `allDocsExclusions`.
2. Expands all documentation artifacts into `allDocsTargetDir`. Which defaults to `docs`.
3. Generates all documentation for all projects. These are copied under `allDocsTargetDir`.
4. Finally, generates an `index.html` in `allDocsTargetDir` that is an index of all the collected documentation.

### Options

Note: the options refer to `name` and this is ill-defined. This is exactly the link text. Easiest to
generate all docs then look at link text to determine `name`.

* `allDocsExclusions` - Set of strings of the `name` for the document artifact to exclude.
* `allDocsRenames` - Map of names to text. This is applied after exclusions.
* `allDocsTargetDir` - Directory relative to root to output. Defaults to `docs`.
* `allDocsSections` - Map of regex to (priority, section text).

EG:

~~~
allDocsSections := Seq(
    "akka-.*" -> (20, "Akka"),
    "log4j-.*" -> (30, "Logging (Log4j 2)"),
    "slf4j-.*" -> (30, "Logging (Log4j 2)"),
    "scala-.*" -> (10, "Scala Standard Libraries"),
    "scalaz-.*" -> (15, "Scalaz Libraries"),
    ".*" -> (999, "Other Included Libraries")
)
~~~

## Testing

Run `test` for regular unit tests.

Run `scripted` for [sbt script tests](http://www.scala-sbt.org/1.x/docs/Testing-sbt-plugins.html).

## TODO

2. [create a bintray account](https://bintray.com/signup/index) and [set up bintray credentials](https://github.com/sbt/sbt-bintray#publishing)
3. create a bintray repository `sbt-plugins`
4. update your bintray publishing settings in `build.sbt`
5. `sbt publish`
6. [request inclusion in sbt-plugin-releases](https://bintray.com/sbt/sbt-plugin-releases)
7. [Add your plugin to the community plugins list](https://github.com/sbt/website#attention-plugin-authors)
8. [Claim your project an Scaladex](https://github.com/scalacenter/scaladex-contrib#claim-your-project)

## Notes

This command may take a while. All dependencies for all compiler targets is, even on small projects,
a large number of files to decompress from jar files. In particular, I've found windows to be
exceptionally slow at this. The recommended pattern is for the organizations CI/CD to deploy this
artifact to a local server.

Failure to generate project documentation does not result in the command failing. The project
documentation will be, however, missing from the index. The first part is intentional. The second
part contains a bug. The goal is to support fetching the docs of new dependencies before the project
even compiles. The issue is any exist project docs should still be included in the index.
