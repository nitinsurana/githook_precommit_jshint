githook_precommit_jshint
========================

It's a sample repository to test pre-commit hook for jsHint in a way that only newly added files are ran through jsHint &amp; commit is allowed based on the result.

```
#!/bin/sh

files=$(git diff --cached --name-only --diff-filter=AC | grep ".js$")
if [ "$files" = "" ]; then 
    exit 0 
fi

pass=true

echo "\nValidating JavaScript:\n"

for file in ${files}; do
	echo "\r\n Validating ${file}\n"
    result=$(jshint ${file})
    if [ "$result" == "" ]; then
        echo "\t\033[32mJSHint Passed: ${file}\033[0m"
    else
        echo "\t\033[31mJSHint Failed: ${file}\033[0m"
		echo $result
        pass=false
    fi
done

echo "\nJavaScript validation complete\n"

if ! $pass; then
    echo "\033[41mCOMMIT FAILED:\033[0m Your commit contains files that should pass JSHint but do not. Please fix the JSHint errors and try again."
    exit 1
else
    echo "\033[42mCOMMIT SUCCEEDED\033[0m\n"
fi
```
