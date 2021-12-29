---
layout: default
---

# Virtual Lab Example

This page lays out how you can write and use your own ChemCollective virtual lab using a simple html and JavaScript webpage that can be hosted on Github pages or any other static webhost. 

<div id="toc"></div>

## Make a new virtual lab

You can create your own virtual lab by forking this repository on [Github](https://github.com/ryanpdwyer/vlab-example) or by using the [CodePen example](https://codepen.io/ryanpdwyer/pen/rNGJBbx?editors=0011), where you can see make live changes to the example lab.

### The virtual lab object

To create and host your own virtual lab, you create a javascript object with the following format.

The virtual lab object should contain the following 6 keys:

1. `assignment`: This informational key should contain
```javascript
{"assignmentText": "Assignment description (displayed when title clicked)"}
```
2. `species`: This key contains information about chemical species. It contains a list of chemical species (in `"SPECIES_LIST": {
        "SPECIES": [list_of_species]`) described in detail below:

    **Species specification**

    *Mandatory keys*

    `id`
    : a unique integer

    `name`
    : Can contain superscripts / subscripts using html tags `<sup>` and `<sub>` respectively

    `enthalpy`
    : (kJ/mol) at 25 °C

    `entropy`
    : (J/mol-K) at 25 °C

    `molecularWeight`
    : (g/mol) should be provided
    
    *Optional keys*

    `density`
    : (g/mL)

    `state`
    : `s`, `l`, or `aq` (assumed aqueous if omitted)

    `simpleName`
    : a simpler/shorter name 

    `specificHeat`
    : (J/g-K)  

    
    *Color keys*


    `colorConcentration`
    : For aqueous species, specify the concentration (M) that gives the color determined by the hue, saturation, and value given (inversely related to molar absorptivity).
    
    `hue`
    : The color's hue (0 to 360)
    
    `saturation`
    : The color's saturation (0 to 100)
    
    `value`
    : The color's value (0 to 100)
    

    **Example** 
    Here is a complete example showing several species:

    ```javascript
    {
        "SPECIES_LIST": {
            "SPECIES": [
                {
                    "id": 0,
                    "name": "H<sub>2</sub>O",
                    "enthalpy": -285.83,
                    "entropy": 69.91,
                    "state": "l",
                    "molecularWeight": 18.016
                },
                {
                    "id": 1,
                    "name": "H<sup>+</sup>",
                    "enthalpy": 0.0,
                    "entropy": 0.0,
                    "molecularWeight": 1.008
                },
                {
                    "id": 2,
                    "name": "OH<sup>-</sup>",
                    "enthalpy": -229.99,
                    "entropy": -10.75,
                    "molecularWeight": 17.008
                },
                {
                    "id": 3,
                    "name": "Fe<sup>3+</sup>",
                    "simpleName": "Fe3+",
                    "state": "aq",
                    "enthalpy": -48.5,
                    "entropy": -315.9,
                    "density": 3,
                    "specificHeat": 0.0,
                    "molecularWeight": 55.845,
                    "hue": 44.0,
                    "saturation": 72.0,
                    "value": 96.0,
                    "colorConcentration": 0.1
                },
                {
                    "id": 5,
                    "name": "FeSCN<sup>2+</sup>",
                    "simpleName": "FeSCN2+",
                    "state": "aq",
                    "enthalpy": 31.25,
                    "entropy": -119.0,
                    "density": 6,
                    "specificHeat": -0.165,
                    "molecularWeight": 113.925,
                    "hue": 0.0,
                    "saturation": 98.0,
                    "value": 54.0,
                    "colorConcentration": 0.001
                }
            ]
        }
    }
    ```

3. `configuration`: Choose what options and models are used:
```javascript
{
  "title": "Iron Thiocyanate Equilibrium", // Displayed in top corner
    "solutionModellers": {
      // This specific heat model adds heat capacity
      // of solids to liquids / solution species
      "specificHeat": "solvent2"
      // There are several other modelling choices available -
      // if not specified, defaults will be used.
  },
  "solutionViewers": [
    // Control tools users have to display solution information
    // Disable display of key tools that would give answers away...
 {"id": "solutionProperties","displayDefault": true,
       "args": {"honorSignificantFigures": false}
    },
    {"id": "aqueous", "displayDefault": true,
        "args": {"unitsToggleEnabled": true}
    },
    {"id": "solid", "displayDefault": true,
        "args": {"unitsToggleEnabled": true}
    },
    {"id": "spectrometer", "displayDefault": false},
    {"id": "particleView", "displayDefault": false},
    {"id": "thermometer", "displayDefault": true},
    {"id": "pH", "displayDefault": true},
    {"id": "vesselTrackingControl", "displayDefault": false}
  ],
  // Allow all three transfer modes:
  "transfer": ["precise", "significantFigures","realistic"] 
}
```
4. `solutions`: This contains all of the solutions available to users:
```javascript
{
  "FILESYSTEM": {
    "DIRECTORY": [
      {
        "name": "stockroom",
        "SOLUTION": [ // List of solutions available
          { // Example solution
                 "name": "1.0 M HNO<sub>3</sub>",
                 "description": "1.0 M Nitric acid",
                 "volume": 0.1, // In liters
                 "species": [
                 {"id": 0 // Solvent can be specified with id only 
                 },
                 {"id": 1, "amount": 0.1 // Other species need amounts in mol
                 },
                 {"id": 6, "amount": 0.1}
                 ]
          },
          {
                  "name": "Distilled H<sub>2</sub>O", 
                  "description": "Distilled Water", 
                  "volume": "3.0", 
                  "vessel": "3LCarboy", // Vessel defaults to 250 mL Erlenmeyer flask
                  "species": [
                     {
                        "id": 0
                     }
                  ]
          }
        ]
      }
    ]
  }
}
```
5. `reactions`: A list of reactions (the vlab will ensure each reaction listed quickly reaches equilibrium at a given temperature)
    
    ```javascript
    {
        "REACTION": [
          {"SPECIES_REF": [
              {"id": "0", "coefficient": "-1"},
              {"id": "1","coefficient": "1"},
              {"id": "2","coefficient": "1"}
            ]
          }
        ]
    }
    ```
    
6. `spectra`: This key contains spectral information (displayed in the spectrum viewer).
```javascript
{
  "SPECTRA_LIST": {
    "SPECIES": []
  }
}
```

### Technical Overview

1. Leave an empty element (div, section, etc...) that the vlab will be loaded into:
  
    ```html
    <div id="vlab"></div>
    ``` 
2. On the html page, load the scripts shown below:
  
    ```html
    <link href="https://nifty-newton-c83258.netlify.app/bundled/911.css" rel="stylesheet">
    <script src="https://nifty-newton-c83258.netlify.app/bundled/850.js"></script>
    <script src="https://nifty-newton-c83258.netlify.app/bundled/526.js"></script>
    <script src="https://nifty-newton-c83258.netlify.app/bundled/911.js"></script>
    <script src="https://nifty-newton-c83258.netlify.app/bundled/lib.js"></script>
    ```

3. Define the virtual lab as a javascript object (see above for details).
  
    ```js
    const data = {
    assignment: {...
    }, ...
    }
    ```

4. Define global variables and load the virtual lab into the empty element with the following lines:

    ```js
    const language = 'en';
    const allowLoadAssignment = false;
    const showFirstTimeTips = false;
    const appModel = new VLab.AppModel();
    const appView = new VLab.AppView({ model: appModel,
                                      el: document.getElementById("vlab"),
                                      vlab: data,
                domain: "https://chemcollective.oli.cmu.edu/chem/jsvlab/"});
    ```

## Example Lab

To view the full javascript for the example below, see [Github](https://github.com/ryanpdwyer/vlab-example/blob/main/docs/vlab-md.md?plain=1#L289). Note that on GitHub pages, there are slight formatting/color issues due to clashing CSS rules-the [CodePen example](https://codepen.io/ryanpdwyer/pen/rNGJBbx?editors=0011) has correct color/font CSS behavior.

<div id="vlab">
</div>

<link href="https://nifty-newton-c83258.netlify.app/bundled/911.css" rel="stylesheet">
<script src="https://nifty-newton-c83258.netlify.app/bundled/850.js"></script>
<script src="https://nifty-newton-c83258.netlify.app/bundled/526.js"></script>
<script src="https://nifty-newton-c83258.netlify.app/bundled/911.js"></script>
<script src="https://nifty-newton-c83258.netlify.app/bundled/lib.js"></script>


<script>
        var data = {
            assignment: {
	"assignmentText" : "<em>Ferric thiocyanate Equilibrium:<\/em>  Using the Virtual Laboratory, analyze the ferric thiocyanate equilibrium using Le Chatelier's principle. This is displayed in help."
},
            configuration: {
  "title": "Iron Thiocyanate Equilibrium",
    "solutionModellers": {"specificHeat": "solvent2"},
  "solutionViewers": [
	{"id": "solutionProperties", "displayDefault": true,
      "args": {"honorSignificantFigures": false}
  },
  {"id": "aqueous", "displayDefault": true,
      "args": {"unitsToggleEnabled": true}
  },
  {"id": "solid", "displayDefault": true,
      "args": {"unitsToggleEnabled": true}
  },
    {"id": "spectrometer", "displayDefault": false},
    {"id": "particleView","displayDefault": false},
    {"id": "thermometer","displayDefault": true},
    {"id": "pH","displayDefault": true},
    {"id": "vesselTrackingControl","displayDefault": false}
  ],
  "transfer": ["precise", "significantFigures","realistic"]
},
            reactions: {
  "REACTIONS": {
    "REACTION": [
      {
        "SPECIES_REF": [
          {"id": "0", "coefficient": "-1"},
          {"id": "1", "coefficient": "1"},
          {"id": "2", "coefficient": "1"}
        ]
      },
	  {
        "SPECIES_REF": [
          {"id": 3, "coefficient": -1},
          {"id": 4, "coefficient": -1},
          {"id": 5, "coefficient": 1}
        ]
      },
      {
        "SPECIES_REF": [{"id": 3, "coefficient": -1},
 {"id": 0, "coefficient": -1},
 {"id": 8, "coefficient": 1},
 {"id": 1, "coefficient": 1}]
      }
    ]
  }
},
            solutions: {
   "FILESYSTEM": {
      "DIRECTORY": [
         {
            "name": "stockroom", 
            "SOLUTION": [
               {
                  "name": "Distilled H<sub>2</sub>O", 
                  "description": "Distilled Water", 
                  "volume": "3.0", 
                  "vessel": "3LCarboy",
                  "species": [{"id": 0}]
               },
               {
                "name": "0.1 M Fe(NO<sub>3</sub>)<sub>3</sub>",
                "description": "0.10 M Iron (III) nitrate",
                "volume": 0.1,
                "species": [
                  {"id": 0},
                  {"id": 3, "amount": 0.01},
                  {"id": 6, "amount": 0.03}
                ]
               },
              {
                "name": "0.1 M KSCN",
                "description": "0.10 M Potassium thiocyanate",
                "volume": 0.1,
                "species": [
                  {"id": 0},
                  {"id": 4, "amount": 0.01},
                  {"id": 7, "amount": 0.01}
                ]
               },
               {
                 "name": "1.0 M HNO<sub>3</sub>",
                 "description": "1.0 M Nitric acid",
                 "volume": 0.1,
                 "species": [
                 {"id": 0},
                 {"id": 1, "amount": 0.1},
                 {"id": 6, "amount": 0.1}
                 ]
               }
            ]
         }
      ]
   }
},
            species: {
    "SPECIES_LIST": {
        "SPECIES": [
            {
                "id": 0,
                "name": "H<sub>2</sub>O",
                "enthalpy": -285.83,
                "entropy": 69.91,
                "state": "l",
                "molecularWeight": 18.016
            },
            {
                "id": 1,
                "name": "H<sup>+</sup>",
                "enthalpy": 0.0,
                "entropy": 0.0,
                "molecularWeight": 1.008
            },
            {
                "id": 2,
                "name": "OH<sup>-</sup>",
                "enthalpy": -229.99,
                "entropy": -10.75,
                "molecularWeight": 17.008
            },
            {
                "id": 3,
                "name": "Fe<sup>3+</sup>",
                "simpleName": "Fe3+",
                "state": "aq",
                "enthalpy": -48.5,
                "entropy": -315.9,
                "density": 3,
                "specificHeat": 0.0,
                "molecularWeight": 55.845,
                "hue": 44.0,
                "saturation": 72.0,
                "value": 96.0,
                "colorConcentration": 0.1
            },
            {
                "id": 4,
                "name": "SCN<sup>-</sup>",
                "simpleName": "SCN-",
                "state": "aq",
                "enthalpy": 76.4,
                "entropy": 144.3,
                "density": 3,
                "specificHeat": -0.165,
                "molecularWeight": 58.08
            },
            {
                "id": 5,
                "name": "FeSCN<sup>2+</sup>",
                "simpleName": "FeSCN2+",
                "state": "aq",
                "enthalpy": 31.25,
                "entropy": -119.0,
                "density": 6,
                "specificHeat": -0.165,
                "molecularWeight": 113.925,
                "hue": 0.0,
                "saturation": 98.0,
                "value": 54.0,
                "colorConcentration": 0.001
            },
            {
                "id": 6,
                "name": "NO<sub>3</sub><sup>-</sup>",
                "simpleName": "NO3-",
                "state": "aq",
                "enthalpy": -207.4,
                "entropy": 146.4,
                "density": 3,
                "specificHeat": -0.333810663,
                "molecularWeight": 62.0049
            },
            {
                "id": 7,
                "name": "K<sup>+</sup>",
                "simpleName": "K+",
                "state": "aq",
                "enthalpy": -252.4,
                "entropy": 102.5,
                "density": 3,
                "specificHeat": 0.133262189,
                "molecularWeight": 39.0983
            },
            {
                "id": 8,
                "name": "FeOH<sup>2+</sup>",
                "simpleName": "FeOH2+",
                "state": "aq",
                "enthalpy": -290.8,
                "entropy": -142.0,
                "density": 4,
                "specificHeat": 0.0,
                "molecularWeight": 72.852
            }
        ]
    }
},
            spectra: {
  "SPECTRA_LIST": {
    "SPECIES": []
  }
}
        };

  const language = 'en';
  const allowLoadAssignment = false;
  const showFirstTimeTips = false;
  const appModel = new VLab.AppModel();
  const appView = new VLab.AppView({ model: appModel,
  el: document.getElementById("vlab"),
  vlab: data,
  domain: "https://chemcollective.oli.cmu.edu/chem/jsvlab/"});
  </script>


<script>
window.onload = function () {
    const headings = document.querySelectorAll("h2, h3, h4, h5, h6")

    const tocVals = Array.from(headings).map((el, ind) => {
      const id = `${el.tagName}-${ind}`
      el.setAttribute('id', id)
      return `<p><a href="#${id}">${el.innerText}</a></p>`
    })

    const tocHTML = tocVals.join("\n");

    const toc = document.getElementById("toc");

    toc.innerHTML = tocHTML;
}
</script>