# Validate State Values


To validate the Salesforce Orgs State Values to the respective list of ISO code and states.

Procedure:
 
**Step 1:**
 Get AddressSetting records through metadata API.
 **USE:**
 File.xml
 `````
 <?xml version="1.0" encoding="UTF-8"?>
<Package xmlns="http://soap.sforce.com/2006/04/metadata">
    <types>
	<members>Address</members>
	<name>Settings</name>
	</types>
    <version>57.0</version>
</Package>
 
 
 `````
 **Step 2:**
 
 Retrive the Data as Single Package, Extract the files then Open Address "Setting" File Which is XML one,
 Use Online XML to Json Converter to conver the XML file to Javascript Object format.
 
 **Step 3:**
 
 The records created through batch run of package, [https://github.com/SaurabhMulay999/statecodes] Package. The Integrationvalues is same as The Label,
 If there is requirement to change the INtegration values as ISO code use JS to bulkily update all the JSON record once.
 
 ````
 let OBJ={}; //Json data which we have converted in previous steps
 for(var i=0;i<OBJ.countriesAndStates.countries.length;i++){
    OBJ.countriesAndStates.countries[i].integrationValue=OBJ.countriesAndStates.countries[i].isoCode;	
    if(OBJ.countriesAndStates.countries[i].states!=undefined){
    for(var j=0;j<OBJ.countriesAndStates.countries[i].states.length;j++){
		OBJ.countriesAndStates.countries[i].states[j].integrationValue=OBJ.countriesAndStates.countries[i].states[j].isoCode;
    }
} 
 ````
Done!!!!!

**Step 4:**

Convert the object return by the above code to XML again and Through Metadata API Deploy Address "Setting" file again, (File should be Zipped)


# Validation part

Validate Wether all Values present in Org by again retriving the data and comparing both ways from org to list and lIst to Org.

````
//Change the Values of OBJ and States as per your requirement.


let OBJ={
/*
In format of 
"countriesAndStates": {
	  "countries": [
		{
		  "active": "true",
		  "integrationValue": "AD",
		  "isoCode": "AD",
		  "label": "Andorra",
		  "orgDefault": "false",
		  "standard": "true",
		  "states": [
			{
			  "active": "true",
			  "integrationValue": "02",
			  "isoCode": "02",
			  "label": "Canillo",
			  "standard": "false",
			  "visible": "true"
			},
			{
			  "active": "true",
			  "integrationValue": "03",
			  "isoCode": "03",
			  "label": "Encamp",
			  "standard": "false",
			  "visible": "true"
			},

*/

}
let State={
/*
In format of
"AT-1" : "Burgenland",
	"AT-2" : "Karnten",
	"AT-3" : "Niederosterreich",
	"AT-4" : "Oeberoesterreich",
	"AT-5" : "Salzburg",
	"AT-6" : "Steiermark",
	"AT-7" : "Tirol",
	"AT-8" : "Voralberg",
	"AT-9" : "Wien",
	"AD-07" : "Andorra la Vella",
	"AD-02" : "Canillo",
	"AD-03" : "Encamp",
	"AD-08" : "Escaldes-Engordany",
	"AD-04" : "La Massana",
	"AD-05" : "Ordino",
	"AD-06" : "Sant Julia de Loria",
	"YE-AB" : "Abyan",
	"YE-AD" : "Adan",
	"YE-AM" : "Amran",
	"YE-BA" : "Al Bayda",
	"YE-DA" : "Ad Dali",
	"YE-DH" : "Dhamar",
	"YE-HD" : "Hadramawt",
	"YE-HJ" : "Hajjah",
	"YE-HU" : "Al Hudaydah",
	"YE-IB" : "Ibb",
	"YE-LA" : "Lahij",
	"YE-MA" : "Ma'Rib",
	"YE-SA" : "Amanat Al'Asimah",
*/
}



let Mpp=new Map(Object.entries(State));
let Mpplist=[...Mpp.values()];
let boolarray=new Array();
let stateList=new Map();
//From ORG to LIST (ISO CODE LIST):
for(var i=0;i<OBJ.countriesAndStates.countries.length;i++){
    //OBJ.countriesAndStates.countries[i].integrationValue=OBJ.countriesAndStates.countries[i].isoCode;
	let ctyCode=OBJ.countriesAndStates.countries[i].isoCode;
	
    if(OBJ.countriesAndStates.countries[i].states!=undefined){
    for(var j=0;j<OBJ.countriesAndStates.countries[i].states.length;j++){
		let stateCode=OBJ.countriesAndStates.countries[i].states[j].isoCode;
		let code=ctyCode+'-'+stateCode;
		//OBJ.countriesAndStates.countries[i].states[j].integrationValue=OBJ.countriesAndStates.countries[i].states[j].isoCode;
		// let val=OBJ.countriesAndStates.countries[i].states[j].label;
		stateList.set(OBJ.countriesAndStates.countries[i].states[j].label,OBJ.countriesAndStates.countries[i].states[j].label);
		//console.log(val);
		// if(Mpp.get(code)=='As'){
		// 	//console.log(OBJ.countriesAndStates.countries[i].label,OBJ.countriesAndStates.countries[i].states[j].label)
		// }
		if(!Mpp.has(code)){
			//console.log(OBJ.countriesAndStates.countries[i].label,OBJ.countriesAndStates.countries[i].states[j].label)
			boolarray.push(OBJ.countriesAndStates.countries[i].states[j].label);
		}
    }
}
}

//checking state and country code (optional)
const keys=[...Mpp.keys()];
const values=[...Mpp.values()];
for(let k=0;k<keys.length;k++){
	if(values[k]=='As'){
		//console.log(keys[k],values[k]);
	}
}
//console.log('StateList',stateList)
//Reverse Comparision from ISO code List(Jeroen) to ORG data
const StateVals=[...Mpp.values()];
//console.log("Statevals",StateVals)
const f=StateVals.filter((f)=>{return f=='As'})
//console.log(f);
const NoState=[];
//console.log(Mpp.values());
for(let x=0;x<StateVals.length;x++){
	console.log(stateList.has(StateVals[x]));
	if(!stateList.has(StateVals[x])){
		//console.log(StateVals[x]);
		NoState.push(StateVals[x]);
	}
}
//console.log(boolarray);
console.log('Values may Not in org', NoState);
//Trying with includes To avoid Small Gaps
let mock=[];
let statelist1=[...stateList.values()];
let Poker=false;
for(var y=0;y<Mpplist.length;y++){
	for(var k=0;k<statelist1.length;k++){
		if(Mpplist[y].includes(statelist1[k])){
			Poker=true;
		}
	}
	if(!Poker){
		mock.push(Mpplist[y]);
	}
    Poker=false;
}
console.log('Values may not in ORG ',mock);




````
