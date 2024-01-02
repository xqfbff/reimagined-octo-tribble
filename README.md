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
f23b61cf-4a56-4a59-9393-27db090c0481
2966e088-3c6e-4f2b-85f3-1c6ec434ceb6
20c45572-8ef4-407d-8e31-c20160864ee8
032a1efd-144d-4fcb-a605-cd06166a7c1a
6b37ffc4-d60e-4a05-9e8a-fd3af9a4d65d
beb77ff6-b455-41f0-8a43-3b83a0b4642f
ae5d9209-e1ed-476d-8819-77e121329325
3abcb486-9f46-47c7-8a2f-6058ee92e72f
a03c2fe2-9c2a-44bd-beb0-97d3d877d452
0ec6e2ac-b926-40d9-84f9-38835fe048cc
40f68f0f-b60c-4e11-9c31-2eae7634dba9
920da348-27ba-4a01-aac3-3d5d1c43177b
3fe9ca9d-9181-4610-ae93-9e4daa5b6f28
3743ab0e-5b6d-4bbd-af45-28919b80746b
be12822a-bbc3-410d-b5d9-ef54d0498530
c5d5b2dc-aaa8-4ffe-8cf4-74e7e77515fe
e5d85c4a-f784-4c5d-9940-16be5da5d845
fc150fcb-78d6-489e-b3b5-754be6b9d696
96529db8-a020-4604-8d8e-ee10a8b91091
12898b2e-44cd-4d54-84de-cacc880539ca
f2683641-8356-49d9-af24-d5144af3a64b
89ab645f-1e6c-4ffb-bb34-48b204cc174d
37434291-2a64-45aa-aa6b-608cb2a24a70
7872a40e-dd10-4a37-ae02-752ffcb6f905
ac6c0ffd-5772-4e20-9000-80de629fba8e
bb1e5eaf-bbd4-455c-a261-cad31b332c07
fcc33be2-21d5-48f1-a4c4-0cc0364b7c89
4edaa907-1341-4c57-b6dd-1a65cf7ec178
6cce7aab-e563-4ac0-a1e4-9420db4f522e
eb348f1e-d231-4b67-a684-5a6bd4c5ed3b
95f25393-638c-4799-8510-e1b900cfe24d
fe0a13ea-d7d3-4323-b282-d2ba329750b3
9ef62de8-e380-435b-8e1e-2f7983094500
5349c620-b0dd-4c6c-bfa8-afeba875fca3
a625221e-1dfc-44d5-9cdb-7c01367ebd85
690cb7e1-d18e-4b7c-a2f5-8cbeca073357
d4d7aefa-0da9-49be-b8de-a241dc103aed
813a28cd-05e1-465e-a0cc-8f53072d9b06
f5c169dd-eeff-4e6e-9428-2d11804ceb39
953305a1-5274-4b12-af6b-109d9572835b
f04dbcc1-68c7-4ba6-9b5b-26a133238414
59109cc3-097c-4060-9aa1-261148cd72ce
a8bc3d4a-856b-4da3-8124-283fe5cbc7f9
9d572e45-fcce-40cc-9606-25fa3f3ef44d
a1269485-6ae4-46b1-afd9-2c416d270761
55cb01c5-3e76-411e-bc1b-9eda11264fa8
182c52ad-7012-4dd0-8ff1-d5e5211d214b
65666247-414c-4145-8774-3d651d2b5cec
e2c24e29-9d46-41b4-8246-711029d03785
d7923003-7a5d-4c0d-837e-c8c055643bfb
cab79f9a-527f-4263-a457-155d52b87244
a825b2ef-10e8-4352-8260-93470aeee35c
763ad86d-7a4a-4357-b625-1a5efd7caa02
b2c115bc-e0d7-4dc8-a861-1e30a91d580d
3d9dc169-0bb2-4b92-9792-3a95d9134a34
fca3d2c1-0b0d-48fd-93d5-36f1439134ac
b6e4aa44-e483-4731-98e7-92d8d834228c
6eaef336-1ed4-45e8-aed5-c239cfbcd3aa
b217c2a3-2ca7-4862-ab75-da26e705ccc6
87af6ead-83bb-4f94-848a-4e66f94b2738
abaa4550-72be-4070-ba25-bc80bf1f646f
b856d00c-198f-44d1-9e9c-cdaecef40ea6
2dba7f1a-60b8-47d7-89c8-02811aa56ee2
d65b3694-0158-4999-a08c-953ef8901e4a
b898b13e-bf61-4a8e-8391-80a3e5c7be7c
b2240565-b5c2-40e2-91ab-e5a70e3bd8c9
b7a28bff-5522-4cb0-af99-11d34f347f8e
7ff2567b-fd66-4b63-9bdb-f77c67819895
c5b56012-ba37-4815-8caf-c7d0397b91ae
e0ca9d5f-cff2-4a84-a197-4fc10741e328
4ea70041-a92f-4a40-a945-ab43af28c72b
47b62b94-c4c1-4700-ba9e-1bdaaad82b0d
19b5f6a7-9629-4fc8-ac7e-6ce68d176ace
c6beb5c9-c207-423e-9cd7-bb756101164a
09730f47-3069-46e0-a1e4-a5a89f3424f9
b37d5e98-b974-4bdc-bc91-185ef318ea4a
3fd84460-8b5a-4c4a-9b29-ae12e1f2a992
11da73d1-b0b8-4d92-bb9e-d67e2043e99b
831eea7f-ca7c-4b00-ac2a-57e7a9de90b5
01d0a6fa-54ee-4c62-8cb1-dbcd85fcdbcb
fe05771e-226d-4be8-842a-1bf84fbf1aa4
b84f9a7d-4402-4004-a69e-59338c887ab7
49075069-097e-40cf-80f3-f1f76497b64e
1b5ba245-16a9-4c0d-bf99-d7fe3fa93924
62c33fd4-9f2c-4695-a2c7-fa1d0759e0a1
df8b03d5-5884-452a-851a-268e50cb872b
09048b3c-8c9a-4362-97ee-9ffa8fcd1808
4967bbd0-55e5-4d89-bf07-b15c9fe8839b
990def0e-d329-45ac-ab05-2a4cd6de8e4a
9a74b293-bd35-46cb-a3f1-b8ff31d1e043
148c8a62-5f20-49f0-8977-bf7dee7cfd6e
f073db7e-f578-484e-8728-a1171d781980
57e46fef-e511-4903-9df8-f36666116e71
190e3f0a-8a96-48ad-bb20-769245f46c5f
c64e1d1a-a57f-4f3d-a554-f9fd35f8aae3
6b0018e4-8e2d-4a4f-b625-75320c753013
ef2b26dd-d2ce-4609-b668-55d99ad43cd4
100d4998-f2d7-4426-8322-8c38bb352962
eec9240b-4198-46be-a7f7-c596e8e71d18
e9774f7f-9a95-456b-a493-c8c8682bbddd
ecc6e4df-6e30-49b2-9b49-aa569a1940cc
2e333c65-2e8c-47cb-b15e-60c555b926fe
9e52c90a-3e77-4330-8e41-7a0bd2c91fc4
03dea7c3-64d7-43a9-90b7-a9573b4b4ba5
d31731a2-2649-4ef7-9eab-6254fee12371
abae78b2-102c-4c7e-8248-9791a708aa57
fe174a58-332e-4660-a061-8465955e2e50
5bfaff4c-7202-4402-a733-e47c98670de5
ac0a5721-34cf-4996-8cb3-06fd72e1d274
db519e3e-65ac-4eac-8806-3bb17709ee58
3cbe76b5-a26b-4017-b30f-56bb43d0dbf2
de743823-2122-4931-a32e-919912a21de8
d379bdcf-e951-4764-9b69-bd6da6ab752a
146eb26a-1751-4930-9e84-f90752fcee2b
fc219011-3dc6-4b1a-90d9-d1f2aca8e16e
4709e993-578f-42e8-9a02-31af196fd4f3
b8201609-975c-48e9-a345-95c78f700175
7af4bdac-44be-46b5-b045-4664ed8e8ba6
02c40390-bc9a-4407-abbd-efb413e86157
adfd2a54-78dc-49d0-b6fc-51599f858f4a
267c9763-052d-4a58-9916-ae9bf00a3e14
79fca228-2f25-4bf0-9374-8f96a7f63f50
6a5cedda-c83f-4784-8331-ce19256f1764
3e155f94-ad08-44b3-8905-b57fdc2a9306
dbbedd08-bc5b-4c63-8300-cce37e128a17
bfa3a32a-b9ff-42a4-b66a-4d754f203589
5a65759d-53f8-41f8-a11b-6cdf4acb966a
a6238a80-93df-4487-b729-746b40860e09
e8048b4f-ccc1-483c-9d2c-a7341b14da23
3a41d76e-8b4a-4656-be75-58f250ed5657
2131e3fc-3fbd-427d-adc4-3b57838ad0eb
3029369f-f221-4f8c-938a-c54717c99e97
a451212c-cc71-4bf7-9352-df5e4e4f4613
b74f2769-5da3-48f3-af5d-7657ff539098
a56b0b61-5f8b-481f-bb1d-1bfe46b193e6
c949c54a-17b1-4eca-b078-126ac19b33f8
417b6e8b-15f7-4cbc-a7ee-8fa884e2b502
6147d24e-a7ed-43fa-a479-34f05fce7495
a552ce1a-dce2-4eea-aec6-6b92757b08a9
63ea6206-9207-474f-910c-ec56265c5dcf
303dbd73-0ca3-4bde-8136-446ca18ee87e
52b0e83f-dd28-4838-931b-7688af776695
db895491-e67f-4470-ba4f-efffe762e7f3
c57b868b-db32-471c-a5fa-445a7aaa7078
f76628cf-a743-4577-91fa-08d5459eea52
58e979f4-03fb-4fbc-9420-c4b8dccb6744
b2c95e53-8a70-47db-b22a-9eb608696086
b166e606-3495-4df7-b646-2e0b62f586de
0dc06e11-5680-4289-90f0-51692d8fd26f
654f9a1d-c2a2-4cf4-937e-2aaaa3c81213
6bd99f0a-b427-4387-9760-7c9fbbbdadd5
00167e0c-2b66-454a-ad1b-517f899508d2
53107b3f-3b84-4498-9f6f-18b271596c0f
0e7a778a-ca43-40dd-8663-26e7c01edbcb
7d05ddda-11b4-47d4-84b1-68f78a7a6a4e
68e66a99-e81d-47f0-9a8c-e2499e37f142
bfe6f450-9ecc-4e74-b3fc-a951e926cd4b
8cbdf53e-dfd2-4ec4-9b5d-b3417bc62379
1c35f497-c62e-4ca7-bbcd-740bff6955db
f0e924cf-0660-4bf3-a0d1-b9093a89884d
ddbd4917-d178-4976-9466-9b31aa487ba5
0812cb01-b732-46c9-9332-3d066c27166f
7cbbad1d-b8cb-44ed-a7b8-eb445871a14f
ccb1cade-fc56-480a-82fe-da187041ca6d
d5347312-9ffb-4bb8-b592-d0e27f1989fc
314d516c-a2cb-4cfc-815d-1a6903d237bb
6b58bd0b-6cf8-4bb0-8fe8-3672eb29a825
f889474a-a1b1-4da7-add3-8244bc0e8200
cc0ec21e-01ad-467f-8ebb-d5623e13b2a0
5f0f720a-dd29-4fd5-9530-af8c09f59175
8ab2985e-e397-49f8-ba6e-43c3ae048a42
690760e4-66d1-4dac-b975-7688f447008a
b6899770-9621-4bea-814b-c594084c9e86
ddabef21-ecd4-4572-be78-b3de3992baa3
8d740ecc-5183-450b-9aec-113ee6bbd261
d7673bf3-68d5-4c1b-ad55-2ebf88d84841
12b60815-b47d-4cbf-a402-4f5850057a4d
87d37e9b-4453-4d22-a19d-eed0c39745b9
49b498e7-fc51-45e7-bd15-98df464d645a
b32d2b72-2d9b-4d1f-a9f0-cfc4b2ff4a53
0fd21915-5a0f-4ec3-b748-8b949d829f58
2ae04ab9-a884-4d90-8dd3-7db6516bd9c0
9506ccb6-191e-4a21-9ee2-0373c3992671
57985537-f6bd-4103-8ba6-daa132976020
d7db6ec4-c618-4419-8df0-3b4f2491d6fd
774d6137-2f7c-49f2-83ca-6c8406d90eb1
a4b9fb8c-71d8-4c16-8f82-9bd7766ecac8
f6b1966f-e7fc-4dc8-910d-a68425ec03cf
2358154f-b369-4ead-986b-d54af4ced335
91cf67e9-ab21-4377-b7a3-0be5856918ca
da02e174-6206-4fd9-8ea3-e6c2ebe211db
9e118070-ac5a-42b8-985d-6d0a8bca3262
7872142c-102e-4f33-be63-ae13a378fc8e
22ed356c-a999-4176-8636-0d9d807e7718
6a78d081-e508-4f2c-b1d3-59246d78d64f
e3fd3129-8538-45a6-a4c6-ce26435baa40
23f67633-631c-4a7a-a6b7-60a522113d0f
e9206a01-a564-4244-b2eb-cab57eacbf03
7a5e048d-afb3-4d97-8c8e-54d6030caab2
740ad2a6-c4cc-4834-8d4f-18e3d673ddea
6cc79e30-e924-436d-8720-926f56402648
eadfdb30-acc2-4765-b0cd-ea5816eac3ee
69e0f433-e511-444c-bae2-93b75fe534f0
c1893f48-1f87-4707-96af-af819f30b050
80512466-5f0d-4fb4-a837-5d6a6cbda30a
348a5abe-f489-49c8-aad4-268c8901907d
01a63dbd-51e8-4861-b1c9-d31703b73367
7e1e38e9-b9d7-4748-b20a-a628d3bd7c81
a845b572-7a7e-4360-9507-d6888b7d9924
057ba50f-814a-44c7-bd6e-29d56cf98bb3
f4bfadf1-e8b4-4847-af34-3450f595ac96
3e27a1e9-aee4-491a-adb8-160ef83fd30e
a6c805d5-39e7-4272-93f8-97619ba8f2dd
457bd965-67b8-4b3d-a34f-fef4d8ec3695
0429124b-3157-4e59-9a98-a0e1865d911b
526d1e3a-2108-4341-8c07-594774f2b430
66012143-778f-4d0b-9db2-780a473d003b
8984fe85-61e5-48ff-b568-e524aa8f0899
f11abeac-d354-40ab-af69-bdf539e441e2
aaf732f0-6c4d-4379-b198-4103335eb721
87495e97-7130-4ad5-af74-027fe22ba8e8
50b48e5d-b828-4481-92d1-9609d4299970
779dd518-dd70-4c28-b1b8-ae886e0e8698
96fa668b-c7d4-4e04-abad-3066eebea4a8
4b560932-07b8-4417-b2b2-242c79b763e9
3e967a43-b55d-40a8-8a78-1fd4ffa4227e
b353e5c9-fe81-4735-aee6-aeb490e89878
10dfb52c-7d55-4adf-b264-409296010644
224aed8a-6e91-40d6-ae43-fa342472ed80
c1fb76c1-c732-4f73-a36c-5e2a5c07d28b
adc595ae-5fc7-4b00-b2e3-cdbab166514f
47d3fef0-4d5d-423c-a55a-a3ffd369d461
1701882f-cc90-4c01-baa5-475823976a38
b007bd8c-40e0-4335-9de5-1d08ed1e8cf8
1bf57f23-6973-4419-bdf7-0a0d07756ab5
dc2f66d5-29ea-4cce-9cc7-089ee6b18bc2
6d8110ef-34cd-4906-b170-7e38d82e601f
f772a663-c6fb-40fb-b31f-3a0e882e1924
7eeefd4e-b935-45b3-9ef0-28341fc4c600
5c8ac43a-f7cb-4241-96a9-a2a40402ee98
6b88cb59-2cf4-4ff6-b6c7-21fa0b3edfa9
04274d05-e2b1-4b1d-93e6-4a92454322df
5c69f4cb-a00d-438d-9a91-d4f9bb9cffca
9ebaa4b7-d4e0-476b-9379-7944a21861f3
bbe96ce7-d9df-4872-8039-56e22d9148a6
9d3f8676-3469-44c0-a0dd-5d0009358b75
40d28810-1335-44eb-92a8-5cef32552830
097b4b7a-edc0-42bf-a301-590d2acf345f
076b0678-e206-4a49-bed2-f368b8d59674
5a9fdcf3-5273-4983-b570-0f755917c893
0cf4fec5-ab17-4209-8515-3ad17f6a4615
316f8fad-8efe-470a-9241-bca9e46809c3
cea30164-489a-4ef1-8b77-ca961c9a9d14
5c00eb09-6c48-44cc-b65d-e7f56ffc462b
bed99064-e25a-4e7b-9982-6d90f6dca848
df2133d7-ed79-4924-ab8e-8b62ab621c2c
75224c8d-5d44-41c5-93ad-36212c62b82d
8a371cb2-1441-4118-b145-ec68325a9ede
22cc25f8-c189-47c4-ab56-564c8b7ef266
3fca6653-019b-4e56-a543-9ab69907995a
c13f82a6-8c71-4cb1-8312-2dba697bf553
2fec5ecc-c6e0-4d95-b49a-5541d82678af
a4f2651f-67d0-4909-a7e3-0a2e4f224ccb
5b132ece-a536-445f-af9d-4d6258108522
5235d5fe-eb38-4ce1-b065-820c6434f091
81ae1e8c-1b04-4e5a-8c22-fdf8a038ee17
3ff047f3-ffc9-4bec-b104-dbee8a1d0fc7
734175aa-607b-4c5b-923e-317b33399e31
21b1b0f3-e73b-4206-88b0-69009fc33767
a573d3fb-33f3-4515-993b-6c1786e44c3b
32b49cff-5d59-4031-846d-13bf4d607599
e8a282b6-1f06-4615-85bd-0b136884d56c
83302b29-ecea-4c41-9b1e-54a42d81351d
ea57bdb3-f801-4a45-9439-6e3a662b451e
6d7d55bc-38e9-4f36-a095-aa2e10fd797e
5ccb84f6-5289-4ff3-85dd-581ed6c358a1
2dfc0231-f927-4be0-8a18-f9ef73cff5c6
59ffbfba-6289-4380-91b2-bdc4692d23b1
82a81dca-6182-4ebb-98b4-20c208d53c71
fa306889-53f2-4382-8fe0-9e788262e4d5
7d414198-b0a4-448b-a384-06b6025ae9f6
4d598603-d9ce-462e-92d6-262df5acfc27
a0c5eb76-2711-4f5c-9cb3-a47b31be2891
48923159-812e-422d-ad7d-1d056a5a9746
46dd208d-b426-454d-ad71-f48b568a6392
2316fa47-2d7b-4a11-a171-233fd4dcf385
9a050819-d1b3-4bd3-a4a5-9356f24b17f0
3ddb6084-0148-40ba-a00e-be7e805b2114
dbf09647-385b-4aba-b299-8c1fb8c1e5c4
1f323f42-cd04-40a5-ba83-201275e00bfc
0429991d-c5b9-456d-b555-2433f2acb394
cec03cca-8bdc-4cd8-a48a-441172864163
6038d4e8-d8e2-4cc4-bc67-32b6ba9e45df
2e30a4ee-9de2-4d44-8fb6-2bbf0dddc7ff
6ad7cc52-dda8-48fc-9b0c-9e1e00f53957
6fe3f574-e89c-48ff-9c98-0c27703c3de4
21b0e6ed-7356-409a-9b24-c649a03ff008
30e95e4e-004c-4305-9b3a-a403d7ef2c3f
99685fba-f141-47a9-b264-925380be6845
8d103596-b855-40e6-8e49-2f3d4d5ae128
fa1e0e5a-1b15-4a34-b1d4-03a3be00bffc
2a328cbb-e168-4519-8c56-cff0a7eb0a9c
9ff3b07b-5a79-468a-82e7-3137dcaaeb91
8e89ea3f-7a03-42b3-8a23-fef616153c0d
3ca4370f-68f8-45d0-953a-6ccafb15a5fd
5bf8ca66-b09d-4adb-b493-06ba5c5d41eb
9d3e05d2-4c2e-40ee-816d-341ab80b950e
75379847-e8d9-4fda-99d0-8f0fda51dcb7
e24457bc-36d0-43c9-935d-865aa88d23af
35408145-b0f6-433d-a76d-27ac8d057abe
474ec488-d5d2-4eda-be5e-895c967c8e64
86980e2b-9144-4f0e-9f01-a9b8d010eed6
27e8eca7-37b0-4c6c-a6c8-d3d2751d3fb5
eb00938f-4259-4231-a25f-3425888e6c77
994a5374-783a-4e40-993a-9ca1388aa705
17a3bac6-9df4-477b-8726-5b43a742ef72
8f361658-ccb0-42ce-be9a-8439a219b155
1aa9e1db-bd68-4c5b-94b8-7fcf130f3414
a356d762-57df-409d-9f3d-82d26440f172
d4091b5a-40ef-4ceb-8f4f-cb56918f26b7
ccfc2d30-bed9-4a3e-945d-9b9125a6e294
8b97abc5-2793-4f0a-9c47-7ef19076a6df
c4d69ee6-2e64-4f9b-9454-e044de435ae5
6cd4abdb-0613-4896-a5b1-9d6f036acb78
92ee4fbd-358c-4d70-81be-2786f717e229
c5ac9e3b-cf68-47d8-a5bd-cd3dd5f5792c
27807fd4-88a3-40e8-a2ff-9b9d49f5e923
0f44d072-f1bb-42b0-8d7c-09fa226ae3e9
f58eca67-be06-4700-b68c-1254351917ee
365cd602-2186-4be2-b413-f17b2f466594
4c427703-2a14-4567-ae23-1e107551fbea
42474767-a1cc-4d43-9b88-f2c870945a04
ddb91ef4-cd66-43be-a883-68392fbb9bd1
f6ed6be9-979a-4808-bc7b-19ab23557d03
d67ad76e-4ec3-4e2c-9997-4b6f3515b905
aeb8215a-6c02-4d76-84bb-f4bc47408c87
26cf0c22-f9d8-4fb2-8a1d-34496d320b7f
944074f3-1d09-425c-b53c-5bd77c3c0a2c
16d69458-ab84-43b9-9c37-f2484dee343b
5f804a72-0877-4b60-8687-72fcad5ad39b
2e6f1b05-7c27-4a55-b06a-0b825394e684
04dc8fdb-d242-4bdc-8665-88842ba4e3ce
6078dafb-6011-4b4f-ba32-9cfb8ce917a1
8be32808-f340-485c-9e36-249e55a4e203
62a94320-f1a5-4dd7-b5d8-949bad1fb5aa
9b45dde7-be88-47a1-954b-c903fd6fd1c0
e429c478-5699-495f-8649-699b54723c00
7304fdfb-cbc6-439e-8c25-19e5c32d8a7a
58e7846e-6be8-42bf-9da1-ad10ff6ff134
045c11e7-cb16-4423-9fbd-24f761bfc08b
e10bd519-21e2-4d7d-8959-34d44cc72f75
c72402b3-906f-4c1a-8cdf-4d477440b1b0
1a38e560-861d-44f4-b027-142bce7c56ce
601b7f8d-d71e-4e37-8f17-f55888b5f418
2fc364ef-7220-450c-9ebe-c411ac797872
a29e5e23-bd84-4eba-9cd6-7e55de0ab878
6c29f429-9184-4890-b03d-805ff6f83b59
5406791c-631a-4bd5-8014-a06e1adf16ea
9fd06375-6054-40f1-b8ce-d5a069a3507a
cf63cd99-7c40-4b57-b6f7-db61c50b151d
02b51fe1-2fec-42e0-8f73-b03fd870dc42
1cc591f6-0a21-4553-9738-ebf83fb6300a
e59a50d5-b872-4a9b-b006-4b1278c2af42
ea4eb821-14df-4f9a-813c-ca7517eb67d8
af041cce-306a-40a4-86e5-7c49f8a4054d
903fa628-9302-445a-b4d4-1e9b7c260510
3b7be3c5-0786-4539-b018-ef40b284d7f5
e6920795-2c79-4398-8e3e-5512c02af42a
f7fcde3d-94e4-49e4-b879-e874f8d52d86
34094381-99e3-41f1-8919-55bfccc6715a
830215ed-d4d4-4c49-91b3-a808db1fc264
bafdddd8-0a86-4395-a41b-785eb3745e32
b855d819-1b2d-4f34-9157-50e05dc9ec93
5bb013d1-a2bf-4003-ab74-8f4df6c24a44
e96def90-5bcc-4b2b-8af4-5f21204edfb1
52b90514-0ea9-402a-bcc1-71f80b3ab49c
6f91ede6-16bf-469e-aad0-7746eefe0ae3
07f4e6c6-fc02-4d7f-8938-a908d4e2c848
783b1a25-c54b-413a-83ee-b0f7ece8809c
718b9b47-bef6-4137-8079-25a517a75381
aa2b1150-4203-4a29-a8a1-28e91ad44b52
054de8b3-fb3d-43bb-b0d3-550e2a1740da
f444db58-e7f0-4c07-a88a-2bf13384ead2
69250827-dfd2-44d1-8e0a-ad356c49c2ea
306c323a-f03c-4b52-bb8a-475b2f10edd3
15af4f31-833f-4cd3-b6d7-7cacda65fd18
a0e5756b-e15b-4376-9d97-6525daa1daf7
24f5de94-2f4b-4184-9f8a-b79e0a12bc9a
cb70d404-ec9b-45cd-be58-ed5181727301
bbb69d83-09d9-4fb4-b3d3-87acc5555374
a20539b0-6249-4248-8a3b-7b93a131e7a9
ec9c97bc-f83b-4372-80fa-8102103324bb
5ff32036-6e0b-4c6e-a811-bdcefd095c91
52555158-1aab-4cde-9949-a2e0db1b7fd1
5bce6ec2-64cd-4080-bb12-f42a27cf5406
a1fc7275-adab-446d-80df-7fa86182d97d
134bdafe-42bb-4a51-aa7e-d0c15000a2ee
350c12f2-336d-4f14-ad5e-fcdaf028d80d
dd47fcc5-ebcd-4abb-9b5f-ecba959b7b1e
a79a0b5d-020c-4563-9f97-5da1e1a32c2a
3e7cfe5a-56ba-435f-8260-3e4b8a183cd2
731083c5-efa6-4c01-a0ff-4233450edde2
56a2acb4-dc31-4b4c-ad78-426a9efff01d
468f77b7-1ce5-4c84-a35f-56ca732b3420
36d406b6-7251-4004-b009-377a14eb416f
1cea80c9-6127-477e-a1eb-507e38786504
67163590-2182-4a69-a31a-a0d8a0580674
3ea6fb7d-bd3f-4515-898a-1e8a39e10ead
847685a8-67f1-4c3d-8e43-c6068913c67d
cc154ae9-a28a-471b-8208-5ca563c0fbe0
9362cb46-2872-40ce-82a3-940bcd75c80a
dbc346ed-543e-4fed-94ee-fcff2beb21df
31cc0860-f83c-4ba7-b1b3-c0e853db762b
8688c3b9-51a1-40cc-8420-dc26804a9bb8
df1d723d-b8c6-4a13-87bd-75f007ce8b26
f79b4e62-e07e-4be9-b5f9-9fffa3b7ee76
dcd69624-2031-49ea-9e58-fc1a7334511d
c2e5cccc-2aa8-4489-9df9-26206f1587af
92b7b57f-8632-40e4-ba61-b77896fe7b46
ac282e2c-b896-4501-8758-027b668caa8e
6ec44e13-6cd8-4036-8b4d-f0105bcc8cdb
8cf0dce1-f84a-4d0d-a673-291cc6b77d76
7b58a15b-6705-4b31-88ee-164575b0141c
29e424b3-c719-45c8-aacd-049d439adf19
7bd16dab-069f-4e3a-85d1-a04fa37d2c39
01319ac4-473d-4d38-921c-9220778982b3
ad40236c-79b5-43bb-a19e-03830c5d385b
24209538-585e-4dd2-8edb-a5643ecf7daf
ef0fc508-5b5d-4290-bc97-3826f6a27b02
52b36835-8223-4cf3-a2eb-801b23ec1cc3
b825ddf0-333b-491a-9898-1660d074d088
1d8d1aa6-35f9-402e-9e52-ec9dad564398
128bf154-f125-465f-b0f9-cc55347a3f61
fbb75401-b551-4dd8-ac97-07e443eadaa9
84a4137c-62d4-4ea7-9921-b9a35e9e7eac
4ef316b9-dd4a-4698-8899-15a42470166a
6e14eea2-eddb-4976-bd98-e68c5e441b4c
e034899d-f638-45d3-9866-25a2a2c09d38
2420dfe9-d547-4700-9565-b772e837a1d7
130c6518-3fc5-48c6-a6bc-d0a307fd63c2
b1535bde-8f42-4fde-9019-c271c0ec55cf
1f632f35-f06e-4bb8-a6c5-b3fea99eab9d
a6e078a4-5a02-43a6-8ea6-1a328c76646c
40486f9e-938f-4ccc-b994-f90996e20b28
f348e4df-de5e-4d07-a583-f9a0aacf72d5
29bce4fc-079e-408d-829e-3a9d1c2e696a
9797d800-71d0-4a0c-9881-e82d15788b0e
ef6a3cf6-f033-4471-8033-db21b42db287
0904b25c-5f87-43f9-8fcc-68689ef25db1
2e364524-08af-4bd0-a374-f3d7cfddf8f1
0902431e-4071-49f0-a5b4-49abaea6a211
7d80ea4f-7768-4c3c-ae08-f0d35ac435a8
cf3d858b-718b-465e-80da-f6c05641c1e5
d5c5563e-0a26-4afb-9093-7f5a48404457
08f79a7b-5f15-448c-acbb-326a4ca7521e
952f3a00-d2d2-4fd6-9c2b-1da19b593bdc
fc1a5b78-7123-4baa-813b-7af4d59da04b
895e1439-3419-4e57-89a0-1884cf3b18cb
29e03487-503f-4585-969f-8522adc5fca0
32f8c7d2-e7d6-491c-b935-5e7144516282
2c2ab079-2836-4d87-82a8-ae0712df0159
8e4be310-dffd-4249-b8d0-565bf73a4dfa
6f0eb7d8-86cf-45cc-8777-448135ea1401
5988eb7e-01ab-4b77-8376-e1f0d17d13cb
cf02b3e8-6167-47c5-97dc-235ccfbb8671
63e8cf34-f8d3-4595-9751-fed0bf7f9bbc
6e0fcb7d-064b-4f82-879a-bd71ee88abc5
7434b49d-8ff8-406c-b05c-9d77a55a4e84
8b200ee6-9020-4628-99dd-21f5a7c70722
b241dce5-5f7a-4369-b3e3-89d8b018b68d
33180262-7307-44c9-9d42-5628ebba7fde
632baca3-9a46-4187-b80b-78d6989333a8
d74bc464-5005-482a-8f59-1122d4f4f013
b6765f8f-7243-4c3e-a6a1-9eea38c1e4d7
dded49b5-105b-4b43-961f-11de09ea286c
a2d04e12-2f92-49b9-8ee3-ac20755620e3
3224abcc-da97-4a14-b28c-855b7412ef63
bd4db856-9ef9-48f8-8390-c577b131ed95
5c3e5166-b0f0-4776-8200-4e64a052dfc0
23719781-8be7-4e21-905d-9c9a427cda96
ccf71258-7548-47ba-9610-11fa930e27f9
8c6b77b8-f49d-4d68-b73b-e0b4066526d7
8210f3c0-0df8-404d-bf01-d2db8a349e8e
eea30e48-9903-4f6f-acfb-ee693919e03e
6c264925-706e-4e11-850f-7493b8490b7c
9af0bf4c-d70d-4d7e-8216-3f80d3097577
2054063c-e561-4206-8dde-2bf71be855d5
2288f63a-3b06-49cd-ac28-d0386069f165
51350b0c-25a7-4186-a658-ba7f5c1f5456
2036dd7e-d3f1-451a-b1e2-5518faec0993
b735a742-c742-4d97-a4f8-5c6d31fab8f6
e8d95eb4-bdd4-4518-bc63-4da382e6fb31
255ae2a5-279f-46a6-986e-1284abc595ee
7a99c586-85c9-494d-8a9e-ef052a72ffaa
fb3287c8-75d7-4d5e-84bf-71be97aa05de
0d59ad6a-a087-4774-9876-c9554f6d8345
f1ce4659-7211-4097-841e-7a643902dd69
1fe793a5-1240-48b5-bc74-8f8ad1c68762
681cb8e6-eef2-4391-bbdf-f449faaee277
10c5a3dc-3f40-4b8b-ad25-71d7fc738c28
b3cde201-ea64-4ebf-bc05-bd18fc25102a
511393b7-ddd8-4536-898b-1fd30dca3a15
5c455a2e-71d9-4402-8d4e-7382fa733f5f
6ddae106-4160-44b4-9927-8c0b702dfb8a
f4e22c86-5180-4329-a2b2-0a95bdd7aadc
74e1bd71-6f96-4ce6-8d77-63929f644377
29180f44-eb6c-416e-9d6e-1fbc1afc52b0
8ee7e780-10ba-47c6-9c87-8c9b49bd44b3
bdecd9c1-44d8-4bfa-bd40-238f005d18c6
ecccdbad-142b-4ddd-996e-2f34da205d7d
852106d2-3ba4-4490-9fe6-d5758690c89e
7b09a61d-7249-4873-8e57-66c443a566a0
e4b59445-fe10-42fe-b932-08fbc02f7280
7b1daaca-6f2c-40ea-93d6-b35768851a07
cdb35a33-9597-4b98-952b-17079ccbffc7
3cf9aded-5777-408f-aa45-64ca7668893c
1a237394-6d59-4585-9b9d-fc8349023a1f
c9bd9c4e-d402-40a3-b0ba-37cb6381106e
96996037-0fa7-48b7-b36a-2999a70b06db
3d669cb2-2fce-4fc2-90e0-44dab1ee22a2
4ab17e67-51a4-4357-a0aa-ea8c3f37c257
ec0af895-73d3-4415-a131-bc4d4aa7415b
32f09e94-6102-450c-b013-679e2cd128d1
d1daecb7-a2ff-460d-b5a0-38ba61fc18c1
fe469e4c-8668-4a85-bf64-0d70889bb884
03732245-8db1-4992-b594-9c3b48266685
c3e174d9-79ee-4ce0-b3a1-06c4e87ee51e
ad76188a-d602-4b3d-983b-3f2dbcde606b
ca77fd1e-daec-4a77-b40a-c3db4afbe143
2e98f4fd-2ebf-4756-9c89-9a72f85b6bcf
42806e6d-4b92-4c5b-bec3-8241c772c505
e32591f8-9808-4170-9a70-2ff62d91127b
3028200c-c92e-40c8-95f5-c73c3a7c071c
360b9381-5bf1-4e59-b405-4097e5a1ed98
d555cdc8-0ee8-4c5f-ac89-bbc9465f3335
930ec857-e2f6-4ab1-b4b5-6acaebb54f78
4c4cc7af-66af-4005-9aca-86ab8090905d
50582581-47ad-4a56-afb9-e3734df39c6c
50053bd6-48eb-4fce-a974-b01c306e8773
a2d8d6f5-2b53-4c89-ac2e-e86e072a5cd8
1a126968-0d62-4a47-8d27-c4a16dc651a6
a3bc3d2c-8fbf-4ace-b59c-6a18dee0d532
259a6be8-a93d-4ea6-a49e-b2bfa1107982
8d4c0917-dd21-409f-b313-47f7d27b9b0f
ec4051e6-fecd-4228-8772-038896381bdc
2bd72cc1-5460-4d85-ad79-299a21df6a6b
e74d3aba-10f1-4345-b6a5-0dd53973c492
c8adc3b4-ac30-41ce-a096-063794b38aa0
60b17607-bb77-4352-ac47-aa6acf0b2a64
70421bbe-feb5-45ed-8ce9-f69382c739f0
87e0da8d-343a-4069-a8ed-48179dc42491
68c6806e-b8b9-4bdf-87d9-48d97e4409fe
880dcce8-bd6f-4cf3-9e09-21997ce58dae
8ee37364-7e48-4851-8433-d4c933cfc913
68cbd2e5-ebad-41de-9527-d11d08b516c4
4201d73e-02d7-4dba-90bf-739315ed9610
9c94fe79-8a01-4436-aab5-7a996e761b0a
d58bede7-47ce-495b-92b0-6b22a709ae9b
81c5448e-0cc1-4cd1-90cf-cdf33c4fc3ba
74e24733-cdf7-4efa-80b8-010ec54d40b4
c8b07df2-61ad-476c-869c-a7954dbdbd09
d20cd7b2-6d6e-456a-968a-4fe16c09067d
2244dacf-5486-4717-91fc-1c68f1c10d09
4b7eb539-b82e-42b3-b0f2-a0545b9aac0f
7bba4242-ee1d-4d63-ad0c-2e32e39297be
37e314a3-c268-49d3-9bb5-73672b0a7ea0
e8a0beea-79ca-441e-9d2a-f745fb5ab531
75a3c4db-e69c-4c57-8166-a90dafe96c54
1c77c8c6-81b2-437e-b60f-747431fcd0f4
bd08340a-95f4-4018-8c23-5fa2c9092c3e
78dd7dbe-ecd1-48d6-bb46-d9e4c44c8dfa
193bb0da-bf2c-4763-97d2-1c04782ddedc
2e0a6132-90ed-442d-b0f3-063e06e44bd6
c8c66c58-c4a6-4bdf-9f03-89d54cf36065
03ab3a5b-aa96-4674-9530-bc7168601912
c3471c66-132a-4aad-b712-58b57008f04a
dba681d5-87b2-4ccb-a7ab-074acbf0d32f
034d4ed8-6adf-45fc-bc32-0603d09d8fe5
a938aac5-0232-4754-9bfd-eb58fe919b67
b1bb543e-f2fb-4458-aead-462a750d0f3f
b3f4120e-cb0e-4f3a-8080-319d538f2747
a52afeeb-2792-43bf-a99f-57322f422dc0
94614028-efbb-4dc8-a9eb-28e151071279
01ecdb01-f00e-4668-9aa4-99cf41bc4760
a04c6ee2-fc7d-4fe4-a298-b8dfa2f3c57c
156b967f-0319-4358-85e0-b9cde4e75b58
851c632f-737d-4546-8245-a7369eadf833
d1dfe18f-2cb5-43f5-9964-11e6d2ce8a45
bdff15b3-a86c-4033-be54-9265b44e0603
48fca542-317c-4b5a-abe1-27d2ae53b0fb
3427d6b9-710c-41bd-bb98-09e47edcf5a9
86459414-5e34-4c8f-8cb6-9c7bb5b7a946
96d847ba-2623-45c3-895e-07794f723029
74e5ca34-67a0-4dda-9224-c1e53d280f2b
4530342a-2b8e-465e-99bf-d4acbe1093e5
bda133cb-0bdb-4ab2-8ae1-4b37a273742b
cad9582b-087d-4653-b6a8-c2caa0e51aed
0c6cc7cf-c6be-43ad-a88b-d6566caea930
250f84cd-5286-48fa-a24a-b5a87a49b61a
f499b7a7-8406-4040-8998-9b576db90554
c0b0aa97-5c80-4e00-8f8c-a96ab0f2f678
202804bb-51b4-48db-8569-142953d9d24f
bd09c0c2-cb02-434c-8bc1-6b851888505a
93ca82d2-a87d-459f-a04d-f2e4a354ced9
e9aa8227-9a9f-41f8-bf45-3151a441cb83
f26423ae-102a-4f0a-9d8c-d4fd31010d4f
2f7b42aa-7d7c-4363-a7ee-499c67df9c55
7011ddb2-748e-48b9-8f11-1197acd760ca
e985ee80-7e8a-4f35-8b64-0e90a8dd769f
3fc77676-44c5-48f6-a169-c03f6974132e
49d2a10e-706a-4302-a88b-c3c514098985
91d12b1f-a3cd-4581-bab9-b8237356e499
41598f4d-c4dc-4f46-b8de-23598c1cd07f
6c399df8-aa16-41b1-98c7-65031f42329e
d0395e2f-6698-4b4d-8abf-9e6ed8d6c0e6
34089ab1-9d77-4a5f-bf92-ced60fb22b22
7ce275c2-3d15-4e4a-adac-ffd6bd38a8db
d9cd07ad-a9fa-4454-8390-9b79d684e0cb
9d2aa26f-1249-44b1-befb-429742e71e3b
25c3bede-b7de-4460-b01c-6acc03934bb0
f8691386-614e-49c3-ba3b-3f4d83c7eeff
bb326215-b37b-41e5-8602-410a389598c8
74414e8a-2817-400a-934b-587747b03fc2
04fa0a74-9cc8-40d7-8fa9-a60f3e1e89be
c1b4e317-8869-48e1-bda0-e9dbcb94043b
9caf3c16-4215-4ac1-b9b9-d5990f1ccf38
b7ab5c5c-d59d-4840-ad44-b0c4d33b58c5
416ced0b-cbb5-4133-8811-cba5ddbac48a