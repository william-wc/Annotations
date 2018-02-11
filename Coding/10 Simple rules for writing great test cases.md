# 10 Simple rules for writing great test cases

[Coding Tech talk](https://youtu.be/U7Ty3Cmr0EU)

## 1 Think before you act

> What are you testing
> Why are you testing

Writing more tests does not improve quality
Code coverage is a quantitative metric only
The tests can only be as qualitative as the code they are testing

## 2 Make your tests understandable

> Comments
> Expected behaviour
> Diagnostics
> Aid debug

What is the relative importance of your tests versus the quality in the code of the product
Tests are just as much as part of the system as the product itself, since you have to understand and maintain both

## 3 Keep your tests small and simple

> Separate test logic/setup
> Use setup/teardown
> Much easier to debug

Making them as small as possible, also makes them simple
Complexity comes from doing multiple simple things

## 4 Test one thing only

> One scenario per test
> Obvious why test failed
> Enables fast debug

Piling multiple assertions and operations mean that the subsequent assertions **need** the previous ones to work
Although it increases certainty level, it does not facilitate the debugging process as a whole

## 5 Fast tests only

> Run unit tests often as possible
> Quick results
> Maintain quality bar

Long tests makes programmers avoid running them often
Keep the feedback loop tight `(write -> fail -> fix -> repeat)`

## 6 Absolute repeatability

> Non-deterministic tests are a headache
> Must trust all tests
> Fix intermittent tests immediately
> No value, waste of resource

Intermittent tests can allow other bugs to pass
Must protect against **false positives** as well as **false negatives**

## 7 Independent tests only

> Must run in any order
> No dependencies
> Run subset, faster results
> Don't modify the system under test

Don't make tests with persistent modifications
Tests of system environments must always clean after themselves
Simply assert what you need, then always cleanup

## 8 Provide diagnostic data on failure

> Use message in asserts
> Reference input data
> Record test environment info
> Make it simple to debug

Failed tests should always provide sufficient information to be able to identity the problem (not necessarily solve it)

## 9 No hard-coding of your environment

> Portable tests
> No ports, IP addresses, data files, databases
> Use config files, system properties or mock objects

Tests should be independent of hardware, operating system, location, owner, ...

## 10 No extraneous output

> A passing test is a silent test
> Too much output = confusion
> Use option, config file to turn on debug, save output

Most of the test cases being simple, should be silent unless failed
More complicated tests like asynchronous operations may need some debugging on the side in case of failure, they don't need to be in the same place as the test logs

## Pirate rules

These are **guidelines** only, but driven by **results**
Some may seem *obvious* or *pedantic*, but until you **inherit** a test suite, you'll never truly understand the true purpose for these guidelines
