1. Program below outputs a value of 0.020000000000000004 instead of expected 0.02. Can you determine what is the reason for this “strange” behaviour? How does arithmetic calculations are performed in JavaScript? What kind of tools would you suggest using for performing calculations which require absolute precision?

Answer: 
Because internally, computers use a format (binary floating-point) that cannot accurately represent a number 0.1, 0.2.
When the code is compiled or interpreted, your "0.1" is already rounded to the nearest number in that format, which results in a small rounding error even before the calculation happens.
I can use Decimal.js to calculate accuracy in javascript.

var a = 0.1 * 0.2
console.log(a) // 0.020000000000000004

// Using Decimal.js
var a = new Decimal(0.1).mul(new Decimal(0.2)) //0.02

2. Rewrite the code sample without try/catch block.
async function getData(req, res) {
  try {
    const a = await functionA();
    const b = await functionB();
    res.send("some result")
  } catch (error) {
    res.send(error.stack);
  }
}

Answer: 
async function getData() {
  const a = await functionA().catch((error) => console.log(error));
  const b = await functionB().catch((error) => console.log(error));
  if (a && b) {
    console.log("some result");
  }
}

3. Rewrite promise-based Node.js applications to Async/Await.

function asyncTask() {
	return functionA()
		.then((valueA) => functionB(valueA))
		.then((valueB) => functionC(valueB))
		.then((valueC) => functionD(valueC))
		.catch((err) => logger.error(err))
}

Answer: 
async function asyncTask() {
  try {
    const valueA = await functionA();
    const valueB = await functionB(valueA);
    const valueC = await functionC(valueB);
    return await functionD(valueC);
  } catch (err) {
    logger.error(err);
  }
}

4. Fix the code. Use any solution (including your favorit library or framework), to make below code more readable.

service.fetchData(function (err,data) {
 if(err){
  console.log(err)
  return
 }
 data.owner = "Master of disaster"
 service.parseData(data, function (err,data) {
   if(err){
     console.log(err)
     return
  }
  service.saveData(function (err,data) {
  if(err){
    console.log(err)
    return
   }
   console.log("data have been stored!")
   })
 })
});

Answer: 
const util = require("util");

const main = async function (service) {
  const fetchData = util.promisify(service.fetchData);
  const parseData = util.promisify(service.parseData);
  const saveData = util.promisify(service.saveData);
	let fetchedData, parsedData, savedData;;
  try {
    fetchedData = await fetchData();
  } catch (err) {
    console.log(err);
    return;
  }

  try {
    parsedData = parseData({ ...fetchedData, owner: "Master of disaster" });
  } catch (err) {
    console.log(err);
    return;
  }

  try {
    savedData = saveData(parsedData);
  } catch (err) {
    console.log(err);
    return;
  }

  console.log("data have been stored!")
};

5. What kind of tools and project structure would you use to design Node.js based microservice code repository. So that future development, maintenance and debugging will only be a pleasure for you and your team mates.

Answer: Seneca
