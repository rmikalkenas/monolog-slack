# Send logs to Slack using Monolog

[![codecov](https://codecov.io/gh/gmponos/monolog-slack/branch/master/graph/badge.svg)](https://codecov.io/gh/gmponos/monolog-slack)
[![Build Status](https://travis-ci.org/gmponos/monolog-slack.svg?branch=master)](https://travis-ci.org/gmponos/monolog-slack)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/2ba4ce7d-cdce-4424-ae6b-deae0107bace/mini.png)](https://insight.sensiolabs.com/projects/2ba4ce7d-cdce-4424-ae6b-deae0107bace)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/gmponos/monolog-slack/blob/master/LICENSE.md)

## Description

This is a package that can help you send logs through monolog to slack using webhooks.
Monolog already has a Slack handler but I am not in favor for these reasons:

**It has some bugs**

- Slack accepts until 2000 chars for [see](https://github.com/Seldaek/monolog/issues/909)

**Performance**

- Although Monolog has the `WhatFailureGroupHandler` I would consider it simpler not to wrap my handler around another
 handler and have a simpler and faster logic [see](https://github.com/Seldaek/monolog/issues/920)
- SlackWebhookHandler does not have timeouts and it executes retries when slack service is down [see](https://github.com/Seldaek/monolog/pull/846#issuecomment-373522968)

**Formatting**

- Current package gives you the ability to add a custom formatter to the handler in order to format Attachments.
Although you can pass a formatter to Slack Handlers of monolog the formatter is applied only to simple messages of slack
and they are not executed for Attachments.
- I like the formatting that I created better than the one that monolog has.
 
## Install

You can install this package through composer

```
$ composer require webthink/monolog-slack
```

## Documentation

### Simple initialization
You can initialize a `SlackWebhookHandler` simple with the following lines:

`$handler = new SlackWebhookHandler('your_webhook_url');`

### Inject custom formatter

Now if you need to pass a custom slack formatter then you need to do the following:

`$hanlder->setFormatter($yourFormatter);`

Note that the formatter passed inside the slack handler must be an instance of `SlackFormatterInterface`.

If you do not pass a custom Formatter SlackWebhookHandler users the `SlackLineFormatter` by default.

## Formatters

### SlackLineFormatter

[](docs/slacklineformatter.png)

## HTTP Client.

### Initialize with a custom HTTP Client.

Inside `SlackWebhookHandler` you can inject your custom Custom HTTP client. The client that is injected must implement
a `\Webthink\MonologSlack\Utility\ClientInterface`.

`$handler = new SlackWebhookHandler('your_webhook_url', null, null, LogLevel::ERROR, true, $clientg);`

## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

1. Run `composer install` from bash.
2. Run `composer tests` from bash.

## Todo

- Use a PSR-18 client instead of a custom one.
