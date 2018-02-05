## BehatHtmlFormatterPlugin

Behat 3 extension for generating HTML reports from your test results.

[![Latest Stable Version](https://poser.pugx.org/emuse/behat-html-formatter/v/stable)](https://packagist.org/packages/emuse/behat-html-formatter) [![Total Downloads](https://poser.pugx.org/emuse/behat-html-formatter/downloads)](https://packagist.org/packages/emuse/behat-html-formatter) [![Latest Unstable Version](https://poser.pugx.org/emuse/behat-html-formatter/v/unstable)](https://packagist.org/packages/emuse/behat-html-formatter) [![License](https://poser.pugx.org/emuse/behat-html-formatter/license)](https://packagist.org/packages/emuse/behat-html-formatter)

### Twig report

![Twig Screenshot](http://i.imgur.com/o0zCqiB.png)

### Behat 2 report

![Behat2 Screenshot](http://i57.tinypic.com/287g942.jpg)


## How?

* The tool can be installed easily with composer.
* Defining the formatter in the `behat.yml` file
* Modifying the settings in the `behat.yml`file

## Installation

### Prerequisites

This extension requires:

* PHP 5.3.x or higher
* Behat 3.x or higher

### Through composer

The easiest way to keep your suite updated is to use [Composer](http://getcomposer.org>):

#### Install with composer:

```bash
$ composer require --dev emuse/behat-html-formatter
```

#### Install using `composer.json`

Add BehatHtmlFormatterPlugin to the list of dependencies inside your `composer.json`.

```json
{
    "require": {
        "behat/behat": "3.*@stable",
        "emuse/behat-html-formatter": "dev-master as 0.1.99"
    },
    "repositories": [
    	{
    	    "type": "vcs",
    		"url": "git@github.com:Eagle-Eye-Solutions/BehatHtmlFormatterPlugin.git"
    	}
    ]
}
```

Then simply install it with composer:

```bash
$ composer install --dev --prefer-dist
```

You can read more about Composer on its [official webpage](http://getcomposer.org).

## Basic usage

Activate the extension by specifying its class in your `behat.yml`:

```json
# behat.yml
default:
  suites:
    ... # All your awesome suites come here

  formatters:
    html:
      output_path: %paths.base%/build/html/behat

  extensions:
    emuse\BehatHTMLFormatter\BehatHTMLFormatterExtension:
      name: html
      renderer: Twig,Behat2
      file_name: index
      print_args: true
      print_outp: true
      loop_break: true
```

## Configuration

### Formatter configuration

* `output_path` - The location where Behat will save the HTML reports. Use `%paths.base%` to build the full path.

### Extension configuration

* `renderer` - The engine that Behat will use for rendering, thus the types of report format Behat should output (multiple report formats are allowed, separate them by commas). Allowed values are:
 * *Behat2* for generating HTML reports like they were generated in Behat 2.
 * *Twig* A new and more modern format based on Twig.
 * *Minimal* An ultra minimal HTML output.
* `file_name` - (Optional) Behat will use a fixed filename and overwrite the same file after each build. By default, Behat will create a new HTML file using a random name (*"renderer name"*_*"date hour"*).
* `print_args` - (Optional) If set to `true`, Behat will add all arguments for each step to the report. (E.g. Tables).
* `print_outp` - (Optional) If set to `true`, Behat will add the output of each step to the report. (E.g. Exceptions).
* `loop_break` - (Optional) If set to `true`, Behat will add a separating break line after each execution when printing Scenario Outlines.

## Screenshot

The facility exists to embed a screenshot into test failures.

Currently png is the only supported image format.

In order to embed a screenshot, you will need to take a screenshot using your favourite webdriver and store it in the following filepath format:

results/html/assets/screenshots/{{feature_name}}/{{scenario_name}}.png

The feature_name and scenario_name variables will need to be the relevant item names without spaces.

Below is an example of FeatureContext methods which will produce an image file in the above format:

```php

     /**
     * @BeforeScenario
     *
     * @param BeforeScenarioScope $scope
     *
     */
     public function setUpTestEnvironment($scope)
     {
        $this->currentScenario = $scope->getScenario();
     }

The following is different to the project this was forked from 

     /**
     * @AfterStep
     *
     * @param AfterStepScope $scope
     */
    public function afterStep(AfterStepScope $scope)
    {
        if (TestResult::FAILED === $scope->getTestResult()->getResultCode()) {
            $counter = $scope->getStep()->getText();

            $scenarioName = $scope->getFeature()->getTitle();
            $fileName = md5(
                    preg_replace(
                        '/\W/',
                        '',
                        $scenarioName . $counter . date('YmdHi'))
                ) . '.png';

            $findSeparator = '/';
            if (DIRECTORY_SEPARATOR !== '/') {
                $findSeparator = '\\';
            }

            /* change your root path here */
            $root = __DIR__ . '/../../';

            $path = $root . 'data/tmp';
            
            $path = str_replace($findSeperator, DIRECTORY_SEPARATOR,$path);
            if (!is_dir($path)) {
                return;
            }

            $this->saveScreenshot($fileName, $path);
        }
    }

```

Note that the currentScenario variable will need to be at class level and generated in the @BeforeScenario method as Behat does not currently support obtaining the current Scenario in the @AfterStep method, where the screenshot is generated

## Issue Submission

When you need additional support or you discover something *strange*, feel free to [Create a new issue](https://github.com/dutchiexl/BehatHtmlFormatterPlugin/issues/new).

## License and Authors

Authors: https://github.com/dutchiexl/BehatHtmlFormatterPlugin/contributors


