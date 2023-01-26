# Testing AngularJS Controllers
## - 1 -
### beforeEach Setup
```
describe("tests", function(){
    
    beforeEach(module('myApp'));
});
```
`module('myApp')` - alias for angular.mock.module
```
describe("tests", function(){

    beforeEach(function(){
        module('myApp');
        setupMock();
    });

    // let's see what goes here in the next part of the code
    ...
});
```
```
...
var $controller;
var mockService;
var myCtrl;

beforeEach(inject(function(_$controller_){
    $controller = _$controller_;

    // setting up a mock (fake) service
    mockService = {};
    mockService.aMethod = function(){
        return 'fake-value';
    };

    myCtrl = $controller('MyCtrl', {MyService: mockService});
}));
...
```
`inject(...)` - alias for angular.mock.inject

`_$controller_` - angular mock injector strips off the '_' 

### Test Method
```
    ...
    it("should update init value on item add", function(){
        myCtrl.addItem();
        experct(myCtrl.value).toBe("fake-value");
    });
}); // end of describe
```

### Include JS Files in SpecRunner.html
```
<!-- include source files here... -->
<script src="lib/angular.min.js"></script>
<script src="lib/angular-mocks.js"></script>
<script src="app.js"></script>

<!-- include spec files here... -->
<script src="spec/MyCtrl.spec.js"></script>
```
## - 2 -
### beforeEach Setup
```
beforeEach(function(){
    module(function($provide){
        $provide.service('MockService', function(){
            var service = this;
            service.aMethod = function(){
                return 'fake-value';
            };
        });
    });

    module('MyApp');
});
...
```
```
...
var $controller;
var myCtrl;

beforeEach(inject(function(_$controller_, MockService){
    $controller = _$controller_;

    myCtrl = $controller('MyCtrl', {MyService: MockService});
}));
```
### Test Method
```
    ...
    it("should update init value on item add", function(){
        myCtrl.addItem();
        experct(myCtrl.value).toBe("fake-value");
    });
}); // end of describe
```