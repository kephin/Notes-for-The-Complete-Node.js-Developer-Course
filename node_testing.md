# Testing in Node

### [Mocha](https://mochajs.org/) as testing framework

:mag: *Mocha is the simple, flexible, fun javascript test framework for node.js & the browser.*

:arrow_down: Install **mocha** by `$ npm i mocha -D`

In the package.json, add `"test": "mocha **/*.test.js"` inside `scripts`. Then you can run `$ npm test` to start the test

Create a folder named **utils**. We will put our testings inside here by naming it **YOURFILE.test.js**

If you want to auto-refresh your test, you can use nodemon by `$ nodemon --exec 'npm test'` in the terminal

We can also add `"test-watch": "nodemon --exec \"npm test\""` inside `scripts` in package.json.

Now, we can just run `$ npm run test-watch`

More features about Mocha:

```javascript
describe('utils', () => {
  const user;
  // run before every test
  beforeEach(() => {
    user = new User({
      firstName: 'Kevin',
      lastName: 'Hsaio',
      age: 30,
    })
  });

  it('can extract it name', () => {
    // assertions...
  });
});
```

### **[Expect](https://github.com/mjackson/expect)** / **[Chai](http://chaijs.com/)** for assertions

:mag: *Expect / Chai lets you write better assertions.*

:arrow_down: Install **expect** by `$ npm i expect -D`

Example assertions:

```javascript
//util.js
module.exports.square = a => {
  if(typeof a === 'string') throw new Error('Only accept numbers.');
  return a * a;
};
module.exports.setName = (user, fullName) => {
  const names = fullName.split(' ');
  user.firstName = names[0];
  user.lastName = names[1];
  return user;
};

//util.test.js
describe('Utils', () => {
  it('should square a number', () => {
    //arrange
    const input = 14;
    const expected = 196;
    //act
    const actual = utils.square(14);
    //assert
    expect(actual).toBeA('number').toBe(expected);
  });

  it('should throw an error', () => {
    //arrange
    const input = 'This is a string';
    //act
    const actual = utils.square(14);
    //assert - test for errors
    expect(function () {
      utils.square(input);
    }).toThrow(Error);
  });

  it('should set first and last name', () => {
    let user = {
      age: 29,
    };
    user = utils.setName(user, 'Kevin Hsaio');
    expect(user).toBeA('object').toInclude({
      firstName: 'Kevin',
      lastName: 'Hsaio',
    });
  });
});
```

:bulb: To test asyncchronous functions, we pass an **done** argument to the test function, and call done() once the tests are finished. Mocha will wail 2s and cut the test.

```javascript
//util.js
module.exports.asyncSquare = (a, callback) => {
  setTimeout(() => {
    callback(a * a);
  }, 1000);
};

//util.test.js
it('should asyn square number', (done) => {
  utils.asyncSquare(15, (square) => {
    expect(square).toBe(225).toBeA('number');
    done();
  });
});
```