# leading-line-remover-loader
A Webpack loader which removes leading blank lines from its source.

## Purpose
We have single .vue files working with ts lint. One rule is multiple blank lines are disallowed by default - unfortunately the vue loaded replaces anything before the script in the .vue file (in our case the HTML template) with a blank line per line removed, triggering the rule. Why the vue loader does this is a different matter - I assume to preserve line numbers in error messages. So this is very much a placeholder fix until the true tooling supports .vue and ts-lint together.

## Usage
First, install the package.
````
npm install https://git@github.com:tombolaltd/leading-line-remover-loader.git#1.0.0 --save-dev
````
Second, include the loader in the webpack rule before (that is to the RIGHT of) the ts linter:
````
{
    test: /\.vue$/,
    loader: 'vue-loader',
    options: {
        loaders: {
          ts: 'ts-loader!tslint-loader!leading-line-remover-loader',
        },
      }
}
````

##Development
The KISS principle has been applioed to this loader, being a single function in a single file with no build or tests scripts, so be aware. 

Note that as this is a loader, it can only work on the source for this file - it cannot determine how many blank lines are there from the the html template (as opposed to "real" blank lines at the start of the code block). As such, if the typescript actually starts with multiple blank lines, these will be removed and missed by the linter, but this is a small price to pay to have the linting otherwise working. Any multiple blank lines other than at the start of a code block and other linting rules should work as normal.
