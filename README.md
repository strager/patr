Promised-based Asynchronous Test Runner (patr) is a very simple, easy-to-use test
runner that support asynchronous JavaScript testing with promises. Patr is based on
the premise that testing should be as simple as creating an object with
methods to perform tests. Objects can be nested to create subgroups of tests.
Patr relies on the system's "assert" module for making assertions. An example test:

    var assert = require("assert");
    require("patr/runner").run({testMath: function(){
      assert.equal(3, Math.min(3, 5));
    });

The suggested pattern for writing test files is to define the tests on the
exports object and running the test runner if the module is the main module.
This allows for direct execution of test files and easy inclusion of tests
into other test groups. For example, we could define "my-math-test.js":

    var assert = require("assert");
    exports.testMath = function(){
      assert.equal(3, Math.min(3, 5));
    };
    
    if(require.main == module)
      require("patr/runner").run(exports);

Now we can directly execute "my-math-test.js" or we could include it in another
test file:

    exports.mathTests = require("my-math-test");
    exports.otherTests = require("other-tests");
    
    if(require.main == module)
      require("patr/runner").run(exports);

With these aggregate test module, each test file's tests are included in a nested object
that will be tested as a subgroup of tests.

= Using Promises for Asynchronous Testing =

Promises make asynchronous testing very simple. You simply return a promise from 
your test to indicate when a test is completed. When using promise-based coding this
is super simple. For example, to test the contents of file loaded asynchronously using
promised-io's fs module:

    var fs = require("promised-io/fs");

    exports.testFile = function(){
      return fs.readFile("testfile").then(function(contents){
        assert.equal(contents.toString(), "expected contents");
      });
    };
    ...

Patr is part of the Persevere project, and therefore is licensed under the
AFL or BSD license. The Persevere project is administered under the Dojo foundation,
and all contributions require a Dojo CLA.