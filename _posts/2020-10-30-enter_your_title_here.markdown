---
layout: post
title:      "Javascript Project"
date:       2020-10-30 14:03:07 -0400
permalink:  enter_your_title_here
---


I did my Javascript Project on an uptodate covid-19 cases around the world..
i got my info from an outside API, a website i found after a few google searches called https://rapidapi.com/.
I found an api with a 100% success rate and with a check mark that reads verified and went for it..
this is a “GET” request snippet from my frontend/index.js file of my Javascript project…
function myApi() {
fetch("https://covid-193.p.rapidapi.com/statistics", {
"method": "GET",
"headers": {
"x-rapidapi-host": "covid-193.p.rapidapi.com",
"x-rapidapi-key": "9d691e50d2msh4aa205c2ff027f5p10f338jsn0e4e7c874dc3"
}
})
.then(res => res.json())
.then(value => {
Search.allCases(value)
;
})
.catch(err => {
console.log(err);
});
}
All the data that comes in is being call in the allCases() function which takes in the data (value) as an argument..
Search.allCases(value)
the allCases() is a static function located in the “frontend/search.js” file aka Search class..( ill explain the Search class later..)
static allCases(value){
const val = value.response.reduce((acc, val) => {
return val.cases.total + acc
}, 0)
h3().innerText = val
}
↑ a snippet from my “frontend/search.js” file
calling static in front of a function is equivalent to calling self
in this case self being the class Search.
im calling the .reduce() method to sum up all the cases to display the sum of all the cases around the world..
Image for post
← snippet from what the allCases() looks like on the website
sidenote: i also did my own css.. even tho i like materialize its not easy to override a lot of things.
snippet of “frontend/style.css” file ↓↓↓↓
#fourth-circle {
width: 200px;
height: 200px;
background: whitesmoke;
                                    border: 3px solid whitesmoke;
                                          text-size-adjust: 15px;
                                            margin: -410px 600px;
                                              margin-top: -500px;
                                            margin-bottom: 200px;
                                              border-radius: 50%;
                                              text-align: center;
}
search.js where every function (including some fetch calls) that belong to Search class is found…
class Search {
static all = []
constructor(country, continent, totalCases, recovered, newCases,
activeCases, newDeaths, totalDeaths, countryPopulation,
totalTests, date) {
this.country = country;
this.continent = continent
this.totalCases = totalCases;
this.recovered = recovered;
this.newCases = newCases;
this.activeCases = activeCases;
this.newDeaths = newDeaths;
this.totalDeaths = totalDeaths;
this.countryPopulation = countryPopulation;
this.totalTests = totalTests;
this.date = date;
}
constructor is where arguments are passed in for info regarding a Search..
this is equivalent to calling self…
if this is called in an static function:
static functionName(){
this = the class value
}
if this is called inside a instance function:
functionName() {
this = the instance value
}
the constructor args are also passed in the backend rails API “searches_controller.rb” file strong params..
snippet from the Rails Covid-19-api 
↓↓↓↓
def search_params
params.require(:search).permit(:country, :continent, :totalCases, :recovered, :newCases, :activeCases, :newDeaths, :totalDeaths,
:countryPopulation, :totalTests, :date)
end
the strong-params is then passed into the create method of the searches_controller.rb file
snippet from the Rails Covid-19-api 
↓↓↓↓
def create
@user = User.first
@search = Search.new(search_params)
@user.searches << @search
if @search.save
render json: @search, status: :created
else
render json: @search.errors.full_messages, status: :unprocessable_entity
end
end
which is also defined and called Search class..
(i get into more detail about this late)
const strongParams = {
search: {
country: country.country,
continent: country.continent,
countryPopulation: country.population,
totalCases: country.cases.total,
recovered: country.cases.recovered,
newCases: country.cases.new,
activeCases: country.cases.active,
newDeaths: country.deaths.new,
totalDeaths: country.deaths.total,
countryPopulation: country.population,
totalTests: country.tests.total,
date: country.day
}
}
this is a snippet of my “POST” request to my backend Ruby rails API ↓↓↓↓
fetch('http://localhost:3000' + '/searches.json', {
method: "post",
headers: {
"Accept": "application/json",
"Content-Type": "application/json"
},
body: JSON.stringify(strongParams)
})
.then(resp => resp.json())
.then(country => {
(country);
})
ps.. a fetch call can be called either in the search.js file (Search class) or the index.js file
you’re prob wondering why i didn’t call a function in front of the data (country) ill explain…
in order to get the info of the Users searches i did another API call to the outside API i found on rapid..
function myApi4(){
fetch("https://covid-193.p.rapidapi.com/statistics", {
"method": "GET",
"headers": {
"x-rapidapi-host": "covid-193.p.rapidapi.com",
"x-rapidapi-key": "9d691e50d2msh4aa205c2ff027f5p10f338jsn0e4e7c874dc3"
}
})
.then(res => res.json())
.then(value => {
Search.search(value);
})
.catch(err => {
console.log(err);
});
}
inside the API call i passed in the data (value) of the API request in-side the search() function.. & since the search function is located inside of the Search class i put Search in-front of it..
Search.search(value)
a snippet of the search() function located in the Search class ↓↓↓↓
static search(value) {
const searchValue = document.getElementById("countryName")
const newValue = searchValue.value.charAt(0).toUpperCase() + searchValue.value.slice(1)
const country = value.response.find(val => val.country === newValue)
if(!country){
alert("Invalid entry")
}
const strongParams = {
search: {
country: country.country,
continent: country.continent,
countryPopulation: country.population,
totalCases: country.cases.total,
recovered: country.cases.recovered,
newCases: country.cases.new,
activeCases: country.cases.active,
newDeaths: country.deaths.new,
totalDeaths: country.deaths.total,
countryPopulation: country.population,
totalTests: country.tests.total,
date: country.day
}
}
const ser = document.getElementById("searchR")
const p = document.createElement("h4")
const p2 = document.createElement("p")
const p3 = document.createElement("p")
const p4 = document.createElement("p")
const p5 = document.createElement("p")
const p6 = document.createElement("p")
const p7 = document.createElement("p")
const p8 = document.createElement("p")
const p9 = document.createElement("p")
const p10 = document.createElement("p")
const p11 = document.createElement("p")
p.innerText = country.country
p2.innerText = country.continent
p9.innerText = 'Population: '
p9.innerText += country.population
p3.innerText = 'Total Cases: '
p3.innerText += country.cases.total
p4.innerText = 'Recovered: '
p4.innerText += country.cases.recovered
p5.innerText = 'New Cases: '
p5.innerText += country.cases.new
p6.innerText = 'Active Cases: '
p6.innerText += country.cases.active
p8.innerText = 'Total Deaths: '
p8.innerText += country.deaths.total
p7.innerText = 'New Deaths: '
p7.innerText += country.deaths.new
p10.innerText = 'Total Tests: '
p10.innerText += country.tests.total
p11.innerText = 'Date:  '
p11.innerText += country.day

ser.appendChild(p)
ser.appendChild(p2)
ser.appendChild(p3)
ser.appendChild(p4)
ser.appendChild(p5)
ser.appendChild(p6)
ser.appendChild(p7)
ser.appendChild(p8)
ser.appendChild(p9)
ser.appendChild(p10)
ser.appendChild(p11)
fetch('http://localhost:3000' + '/searches.json', {
method: "post",
headers: {
"Accept": "application/json",
"Content-Type": "application/json"
},
body: JSON.stringify(strongParams)
})
.then(resp => resp.json())
.then(value => {
(value);
})
resetInputs() ← (explained later)
}
as you can see im calling the fetch request inside of the search( ) function and since strongParams (which holds the value of the variable country ) is defined inside of the function where the fetch call is located there was no need to call a function….
snippet form "frontend/style.css" 
↓↓↓↓
#first-circle {
width: 500px;
height: 500px;
background: whitesmoke;
/* background: rgba(255,255,255,0.5); */
border: 3px solid whitesmoke;
text-size-adjust: 15px;
margin: auto;
margin-top: 200px;
border-radius: 50%;
text-align: center;
margin-right: 470px;
overflow: auto;
background-image: url(img/earthcov.jpg);
background-size: auto;
background-attachment: auto;
}
snippet from the website of search()
↓↓↓↓
Image for post
The first line of code in the function
     
const searchValue = document.getElementById("countryName")
is grabbing the input buy the id name and setting the variable searchValue equal to the it.
snippet from frontend/index.html file ↓↓↓↓
 <input type="text" id="countryName" name="country-name" class="input" placeholder="Country Name..">
.
the the second line of code in the function 
const newValue = searchValue.value.charAt(0).toUpperCase() + searchValue.value.slice(1)
is making sure every input is capitalized (so whether you put in china or China it still gives you Chinas covid-19 cases info)
although there is a slight flaw.. 
*if all caps were put in it would throw an error and that is why we have the forth line of code 
Image for post
← website snippet 
if(!country){ alert("Invalid entry")
}
which reads if the country isn't in the database alert saying "invalid entry"
.
the third line of code in the function 
const country = value.response.find(val => val.country === newValue)
↑↑↑↑
// here were iterating through the value (which is being passed in as an arg in the search(value) and getting the individual country and setting it equal to the value that is being entered inside of out search with the .find() method.
The find() method executes the function once for each element present in the array: If it finds an array element where the function returns a true value, find() returns the value of that array element (and does not check the remaining values) Otherwise it returns undefined.
so all searches that were made on the site are pushed to the rails API…
I did a couple of more fetch “GET” calls to the API i found online for more info.. and added two more functions allDeaths() && allRecovered() to post at the top of the page along side the allCases().
Image for post
Each of these functions are located inside of the Search class and each have their own API call that they're getting their values from... which is identical to the first myApi() function i posted in the beginning of this blog...
snippet from the website  ↓↓↓↓
Image for post
snippet from search.js file  ↓↓↓↓
static allDeaths(value){
const val = value.response.reduce((acc, val) => {
return val.deaths.total + acc }, 0)
h32().innerText = val
}

snippet form style.css file ↓↓↓↓
#third-circle {
width: 200px;
                                          height: 200px;
                                             background: whitesmoke;
                                       border: 3px solid whitesmoke;
                                             text-size-adjust: 15px;
                                               margin: -410px 900px;
                                                      margin-top: ;
                                              margin-bottom: 200px;
                                                 border-radius: 50%;
                                                 text-align: center;
                                                                }
snippet from the website  ↓↓↓↓
Image for post
snippet from search.js   ↓↓↓↓
static allRecovered(value){
const val = value.response.reduce((acc, val) => {
return val.cases.recovered + acc
}, 0)
h33().innerText = val
}
snippet from style.css     ↓↓↓↓
#second-circle {
                                                       width: 200px;
                                                      height: 200px;                  
snippet from index.html                      background: whitesmoke;
↓↓↓↓                                   border: 3px solid whitesmoke;
                                             text-size-adjust: 15px;
<div id="second-circle" >                      margin: -700px 280px;
<h4 id="totalR">Total Recovered<h4>                    margin-top: ;
<h3 id="allRecovered"> </h3>                   margin-bottom: 200px;
</div>                                           border-radius: 50%;
                                                 text-align: center;
                                              background-size: auto;
                                                                }
they're also doing the same exact thing as the allCases() by using 
the reduce() method to sum up all the info.
- allDeaths() summing up all the deaths caused by Covid-19 across the globe
-allRecovered() summing up all the recovered cases form Covid-19 across the globe 
to get User information i did a couple of “GET” calls to the backend rails API .
*Recent searches* && *the most viewed country*
snippet from the website
↓↓↓↓
                     *the most viewed country*
Image for post

snippet from style.css 
↓↓↓
                                         #sixth-circle {
                                           width: 650px;
                                          height: 150px;
                                   background: whitesmoke;
                             border: 3px solid whitesmoke;
                                   text-size-adjust: 15px;
                                           margin: 380px ;
                                       margin-top: -350px;
                                       border-radius: 50%;
                                       text-align: center;
                                      margin-right: 260px;
                                         overflow: scroll;
                                                     }
snippet from index.html 
↓↓↓↓
<div id="sixth-circle" >
<h4 id="userMostViews">Most Views..</h4>
<h3 id="mostViews"> </h3>
</ul>
</div>
snippet from search.js 
↓↓↓↓
static mostVs(data) {
const country = data.map(da => da.country)
let x = 1;
let y = 0;
let mostViewed;
for (let i=0; i<country.length; i++)
{
for (let j=i; j<country.length; j++)
{
if (country[i] == country[j])
y++;
if (x<y)
{
x=y;
mostViewed = country[i];
}
}
y=0;
}
const mostViews = document.getElementById("mostViews")
const p = document.createElement("p")
p.innerText = mostViewed
mostViews.appendChild(p)
} 
}
the last few lines of code are doing exactly what they say.. 
grabs an element my its id name
const mostViews = document.getElementById("mostViews")
creates the element <p></p>
const p = document.createElement("p")
adds the mostViewed variable which is country[i]
into the <p></p> element
p.innerText = mostViewed
adds the <p></p> element and the value inside it into the variable mostViews which is the html with the id of mostViews
<h3 id="mostViews"> </h3>
mostViews.appendChild(p)
mostVs(data) {
the code in the function mostVs(data) looks complicated but it's pretty simple
* it iterates through each element in the array comparing it to see * if the elements are identical 
* it adds one to the x variable 
              and 
* if x is higher than y
* it sets the y variable to the value of x
              and 
* sets the variable mostViewed to the element that is being compared.. 
* the element with the highest repetition wins 
}
.
snippet from the website
↓↓↓↓
                         *Recent Searches*
Image for post
←
←
snippet from style.css 
↓↓↓
#fifth-circle {
                                           width: 650px;
                                          height: 150px;
                                 background: whitesmoke;
                           border: 3px solid whitesmoke;
                                 text-size-adjust: 15px;
                                         margin: 380px ;
                                      margin-top: 700px;
                                     border-radius: 50%;
                                     text-align: center;
                                    margin-right: 100px;
                                       overflow: scroll;
                                                     }
snippet from index.html 
↓↓↓↓
</div>
<div id="fifth-circle" >
<h4 id="user">Recent Searches..</h4>
<h3 id="userInfo"> </h3></div>
snippet from index.js
↓↓↓↓
static recentSearches(data) {
const userCountry = data.map(da => da.country)
const userInfo = document.getElementById("userInfo")
const p = document.createElement("p")
p.innerText = userCountry
userInfo.appendChild(p)
}
recentSearches(data) {
the block in this function iterates using the map() method and finds all the countries.
sets the method to the variable userCountry and inserts it into the 
<p></p> thats created.. const p = document.createElement("p")
which is set to the variable p 
then adds the variable p to the userInfo variable 
const userInfo = document.getElementById("userInfo")
which is grabbing the html element by its ID name.. 
<h3 id="userInfo"> </h3></div>
it then adds the p variable (which hold the value of userCountry)
to the userInfo variable
userInfo.appendChild(p)

}
below is the event listener DOMContentLoaded which loads all the functions within it as soon as the DOM loads.
document.addEventListener("DOMContentLoaded", function(){
onCall()
myApi()
myApi2()
blink()
blink2()
myApi3()
blink3()
getRails()
Search.userInfo()
})
onCall() holds event listeners within its block… to listen to an( “event”, theFunction)
snippet from index.js
↓↓↓↓
function onCall() {
btn().addEventListener("click", changeColor)
form().addEventListener("submit", submit)
}
function changeColor() {
btn().style.color = "green"
}
if you're wondering what button is its a global function inside the index.js.. (ill list them all later)
function submit(e) {
e.preventDefault()
myApi4()
}
this function prevents the page from reloading with the event of the "submit" key being pressed with the block
e.preventDefault()
the function myApi4() hold the submit button with in it 
function resetInputs() {
const searchValue = document.getElementById("countryName")
const ser = document.getElementById("searchR")
searchValue.value = ""
}
this function empties the search bar after an input is submitted.
which is being called at the end of the search() function 
this is a list of all my global functions..
const btn = () => document.getElementById('btn')
const totalR = () => document.getElementById('totalR')
const totalD = () => document.getElementById('totalD')
const totalC = () => document.getElementById('totalC')
const h3 = () => document.getElementById('allCases')
const h32 = () => document.getElementById('allDeaths')
const h33 = () => document.getElementById('allRecovered')
const form = () => document.getElementById("find-cases")
const allInputs = () => document.getElementById("all-inputs")
const searchValue = () => document.getElementById("countryName")
i also added a blink() function if you noticed in the event listener DOMContentLoaded
document.addEventListener("DOMContentLoaded", function(){
onCall()
myApi()
myApi2()
blink()
blink2()
myApi3()
blink3()
getRails()
Search.userInfo()
})
these functions are called on the allCases(), allRecovered(), && allDeaths() and do exactly as their name make the data blick
they’re all identical so ill only list one
function blink() {
const h3 = document.getElementById('allCases');
setInterval(function() {
h3.style.display = (h3.style.display == 'none' ? '' : 'none');
}, 1000);
}
*this function grabs the element my its ID..
* calls the method setInterval()
and sets the 1000 arg at the end that controls the timing of the blink
thanks for reading :)
