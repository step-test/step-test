# Step Test
This framework operates much like cucumber but is class based. 
```
test = new StepTest()
test.name = "My Test"
``` 
Is the same as 
```
StepTest.test("My Test")
```
This means you can use class inheritance, and can change the class prototype. Any shared test data can also be placed on your current instance. Every instance method has access to ```this``` which is how one can attach ```scratch``` like so ```this.scratch```

#### addStep vrs Step
* addStep {Class Level} - Tests are broken up into reusable steps and attached to to the class
* step {Instance Level} - Steps are then stacked up with expectations to create the test.

#### Parallelism and Isolation
All testing is done in parallel so don't write tests that rely on each other, each test must be designed to operate independently.

### Setup
```
import StepTest from 'step-test';
```

### Steps
```
StepTest.addStep("Mount UserComponent", function(){
  render(UserComponent, this.scratch)
})
.addStep("Enter Email", function(){
  this.scratch.querySelector("#email").value = "here";
})
.addStep("Enter Password", function(t){
  this.scratch.querySelector("#password").value = "here is password";
})
```

### Async Steps
```
StepTest.addStep("Test Async", function(){
  var t = this.defer()
  setTimeout(function(){
    // Do something
    t.resolve();
  }, 1000)
})
```

### Tests
```
StepTest.test("Awesome")
.step("Mount UserComponent")
.step("Enter Email")
.step("Enter Password")
.step("Test Async")
.expect("Should Render UserForm", function(){
  return this.ok(t.scratch.innerHTML == "<div></div>"); // Chai can be used and it is suggested
})
.play()
```

### Debugging
You may have noticed that ```.play()``` is available, ```.pause()``` is also available, and ```.next()```. These functions can be run in a step and give you control over when each step advances, this strategy can be extremely helpful when trying to find the offending step as you can keep checking your test and then running ```.next()``` until you find where things went wrong.
