## PHPStan issue: inline ignores of collectotor-based rules causing false positive of unmatched ignore

How to reproduce:

```sh
composer install
cat src/Foo.php # check incriminated file

vendor/bin/phpstan # no errors as expected
vendor/bin/phpstan analyse src/Foo.php # unmatched ignore error
```

This is due to the fact that dead code analysis needs full codebase analysis to run properly. 
Thus it [uses](https://github.com/shipmonk-rnd/dead-code-detector/blob/bbc92af/src/Rule/DeadCodeRule.php#L143) `if ($node->isOnlyFilesAnalysis()) return []` which causes this issue.
