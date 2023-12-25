# has.js

Pure feature detection library, a la carte style.

This document is **not** complete.

## About

Browser sniffing and feature inference are flawed techniques for detecting browser support in client side JavaScript. The goal of **has.js** is to provide a collection of self-contained tests and unified framework around using pure feature detection for whatever library consumes it.

You likely _won't_ see **has.js** as a public API in any library. The idea is that _%yourfavoritelibrary%_ will import some or all
the available tests, based on their own needs for providing you a normalized future proof API against common tasks.

**not stable**, so keep that in mind. This is a young project, and the decided naming conventions may be a moving target. The tests are nothing that haven't been done over and over in various places, so the intention is to come to some agreement on a basic naming convention and API based on real world use cases.

Currently, the testing convention is `has('somefeature')` returns _Boolean_, e.g.:

    if(has("function-bind")){
        // your enviroment has a native Function.prototype.bind
    }else{
        // you should get a new browser.
    }

In the real world, this may translate into something like:

    mylibrary.trim = has("string-trim") ? function(str){
        return (str || "").trim();
    } : function(str){
        /* do the regexp based string trimming you feel like using */
    }

By doing this, we can easily defer to browser-native versions of common functions, augment prototypes (which **has.js** will _not_ do) to
supplement the natives, or whatever we choose.

Running `has()` is a one-time cost, deferred until needed. After first run, subsequent `has()` checks are cached and return immediately.

## Testing Registration

Each test is self-contained. Register a test with `has.add()`:

    has.add("some-test-name", function(global, document, anElement){
        // global is a reference to global scope, document is the same
        // anElement only exists in browser enviroments, and can be used
        // as a common element from which to do DOM working.
        // ALWAYS CLEAN UP AFTER YOURSELF in a test. No leaks, thanks.
        // return a Boolean from here.
        return true;
    });

You can register and run a test immediately by passing a truthy value after the test function:

    has.add("some-other-test", function(){
        return false; // Boolean
    }, true)

This is preferred over what would seem a much more effective version:

    // this is not wrapped in a function, and should be:
    has.add("some-other-test", ("foo" in bar)); // or whatever

By forcing a function wrapper around the test logic we are able to defer execution until needed, as well as provide a normalized way for each test to have its own execution context. This way, we can remove some or all the tests we do not need in whatever upstream library should adopt _has_.

## Platform Builds

Something resembling a "builder" is coming. A basic dependency matcher and test lister is provided in `build/`.

## Contribute

**has.js** contributions are covered under a common license by way of [Dojo Foundation CLA](http://dojofoundation.org/cla), and brought to you by the following awesome folks:

  + [John David Dalton](http://allyoucanleet.com/) - FuseJS Project Lead
  + [Brad Dougherty](http://github.com/bdougherty)
  + [Bryan Forbes](http://http://www.reigndropsfall.net) - Dojo Committer
  + [Ryan Grove](http://twitter.com/yaypie) - YUI engineer
  + [Andrée Hansson](http://github.com/peol)
  + [Peter Higgins](http://higginsforpresident.net) - Dojo Project Lead
  + [Paul Irish](http://paulirish.com) - jQuery Team, Modernizr developer
  + [Weston Ruter](http://weston.ruter.net/) - X-Team/XHTMLized developer
  + [Rick Waldron](http://github.com/rwldrn/)
  + [Juriy Zaytsev](http://perfectionkills.com) - @kangax, nuff' said.

There is an _IRC_ room setup for discussion or questions: **#hasjs@irc.freenode.net**

## Conventions

Internally, we follow these conventions:

  + All Strings are quoted using double-quotes `"`
  + Test names are lowercase, hyphen separated strings. Enclosed in double-quotes
    + Tests are passed `g`, `d`, and `n`. Use these aliases always.
  + Globals are as follows, available as used but will be reduced to a single ref:
    + `STR == "string"`
    + `FN == "function"`
  + Tests return Booleans. Sometimes, you must coerce a boolean:
    + DO return `!!(someExpression)` as necessary
    + DO N0T return `!!("x" in y)` or anything else that would otherwise return a boolean, e.g.
      + `x !== y`, `x > y`, `typeof y`
    + DO wrap expressions in parens: e.g. `return ("x" in y)`

## License

Tentatively, **has.js** is available under the Academic Free License, New BSD License, and the MIT License. Providing this common code under multiple licenses requires us to have all contributors agree to a [CLA](http://dojofoundation.org/cla).

## TODO

**has.js** is open source, and open to contribution. Please fork and send pull requests as you see fit. This is a rough list of things that are needed or coming:

  + moar tests. Fork/pull request anytime.
  + Static Frontend - some home to put a static instance of has.js online to collect UA -> has(test) mappings
  + Documentation regarding each of the tests, by name. eg: `has("native-xhr") // tests if the environment has a native XmlHttpRequest implementation`.
  + moar tests. Again with the forking.
  + "compiler" code / frontend
     + ideally something that will use the list of tests, provide a clean interface to selecting tests needed and to download a single has.js file with tests embedded.
     + keeping in mind to remove additional closures and provide (only needed) `var CONTS = ""` style helpers in a single wrapping function.
567a6012-44b3-4f82-bf0e-6e7148178405
2ae0e1bd-1c80-4093-ae64-cbce730b6f26
385c386e-528f-49ba-be4d-af547972afc1
d88fffe7-ecb2-476e-a25c-8667b53c5aed
f1377322-9232-4572-8ac9-d8836c73bb90
03c0daa9-71ba-4d01-8875-bf1547a6a554
5fba3217-19d4-4ede-a8e0-d6983c7d595a
7a97b32c-c1c8-4a6b-bb0f-89267e441060
2a3ef04a-7770-4332-881d-8660cbde6952
a8c4eaaf-8c0a-4ab9-9cfd-6d7029b529ca
740eefa6-f9d2-43d3-ba52-afe289a6a5e2
52b319b4-4cf6-42a3-88eb-a6450225fbb0
6f484e82-540f-4e41-a530-12edba30d356
28e0af5e-dafd-4fa2-9b01-55d410d10767
44b63d09-10e9-4435-b8ee-9df6142fce42
7516a7e7-fa9a-46f4-a7f2-aead8f2b623b
ca86440e-90b8-4861-bfae-59cfea9addd5
7d2ce788-84b1-4199-b490-589535b07141
f2d2fdbe-7ff0-4654-9387-fb14862dfeb3
d0334f55-54b6-4d7d-a559-ab23d60cd9ce
8e2af19f-1869-4bdd-acd6-c4eb6636dc0f
b952273c-bfac-4455-8909-e1a28930e7a1
a855b015-01e8-41be-8a35-d3756c000e4a
6fd83fca-f6b4-4ce9-85f2-65b13bf79ab5
55f3b4b3-0685-4503-a876-80dc49853719
48cafb4e-73ff-410a-8773-d53a75d3fedc
dbbb53f3-df9e-48c7-96d2-6632bd55bcf5
008660a1-0ee9-4c32-a428-564cb39b099d
6f236b71-4f0b-4c3a-9c29-5a79fdde1750
d45750f2-5946-4d71-b0cd-09ecdd4aa24d
abfd01cc-239e-463f-829c-313f57ec8294
1ada4ad2-65b5-43f2-ae5c-6e81696af19b
ed05a851-2558-427c-a448-5a6f4f64e212
665d561a-e6ae-4891-b1fe-33ae828294b1
a8e72101-b103-422b-b6c0-c9455a94324c
daf5baaa-b282-4e12-bd38-d6db4a2e911e
e9966ea9-2564-4555-950d-b6371654bbe6
833c3b0a-e786-43e2-83ce-b884f634711b
adbc961b-785b-458c-9af4-c40a0077254d
a4767e17-709f-462a-901f-9f473e533ec3
7f577287-8e88-4310-a5e0-4c5fcc2290bd
5fa83dfa-147c-45b2-9606-458324abc7a5
15d692a9-b468-47b4-b981-a44d23471b42
49331e85-d3e2-40b2-b6c6-8b0484033237
8b1ed359-99dc-4bfa-874c-8a40966a41a5
07836b23-cf01-4911-a374-63625849c9cf
dbe1feb2-73d7-48e0-95b0-16dfef367dda
cc63a61d-c971-4929-a8d7-9862e86480ee
34486389-a81a-4bbf-8f75-0e9420237df3
7968a104-1504-410a-9d9a-affb7a8b5519
2037ae13-0bab-4ab3-9dd9-57a6084b9b18
9d1fd618-6e1f-4037-a123-e1aaf4a4b35a
b45fa1aa-6c3e-4152-8f59-2f0c452c9a8e
46870c4f-96af-42d4-947c-e0067aa61b34
18bd0c76-d2ca-4148-b4c1-989695775bec
60a8d808-69b3-43e8-afec-c794a3fc7911
3def2fb7-4522-432f-be64-659d0ddbea83
6f0ab709-b51d-4c5b-951e-55c1e2c806f6
19164b7f-3e8b-467d-8d09-4a783041f01f
46d9b1f4-6669-40a3-841b-a7cc7f825511
ee698d4b-832b-4459-98a6-950c1a3b3e36
8d2f6bf6-b490-4faa-a56d-ac4bb7d35c47
99f3480a-b357-493c-abad-01bf84aa5459
e91a5d1f-d89a-4deb-a7d5-4392d5b083d6
09b2d762-81cf-4a9e-8fa0-8f2270010c64
09acd70e-7b29-457d-b24e-7f0bbae2313e
a1448f6a-be52-4127-91ac-84f7d2e0bd86
ca97cf19-4d60-42ba-bbd2-91a9c962586e
eaf90eb8-7d12-4ead-8980-c264105c54b2
7797039f-4d56-48b4-861d-f8f9e6b744cd
683444ef-e40c-426f-9b47-c23b39b35558
3dd07d63-9d2f-4c18-a6a1-56f5782d70c8
0d04a4c4-79ba-4739-ad0c-5fda1f4be4b8
c3b51d9b-375b-4808-8f0e-aba12000435d
3d5048b2-0c94-4e7d-b14c-d6eb2eb1ab43
60fb1487-13b2-4b39-bc8e-b5ea21e8b353
6400ee41-02b4-4e8c-bc0a-7bf579b69bec
79f686fe-8a97-4e5b-b4b5-6017f37aae7d
7dabdbaa-c61b-4cac-8f8a-d46554c473f6
f9a4eff4-16b7-4714-90f1-951d8b5c173f
c0f7b3ff-5825-43f6-8960-eb2e1955c993
da82b328-1e20-4871-8389-15cf6c49b274
9f03d3a2-e41b-4ce9-bf6b-483ffb500c86
fcf48c57-43a7-449f-930e-7a94a03c564a