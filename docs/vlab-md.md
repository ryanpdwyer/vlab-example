# Virtual Lab Example



## Make a new virtual lab

You can create your own virtual lab by forking this repository on Github and following the steps below.

<div id="vlab">
</div>



<script src="https://chemcollective.oli.cmu.edu/chem/jsvlab/bundled/lib.js"></script>
<script>
        var data = {
            assignment: {
	"assignmentText" : "<em>Potassium Permanganate Titration:<\/em>  Using the Virtual Laboratory, design and perform a titration to determine the concentration of the unknown KMnO<sub>4<\/sub> solution to four significant figures."
},
            configuration: {
  "title": "Iron Thiocyanate Equilibrium",
    "solutionModellers": {
      "specificHeat": "solvent2"
  },
  "solutionViewers": [
	{
      "id": "solutionProperties",
      "displayDefault": true,
      "args": {
        "honorSignificantFigures": false
      }
    	},

        {
      "id": "aqueous",
      "displayDefault": true,
      "args": {
        "unitsToggleEnabled": true
      }
    },
    {
      "id": "solid",
      "displayDefault": true,
      "args": {
        "unitsToggleEnabled": true
      }
    },
    {
      "id": "spectrometer",
      "displayDefault": false
    },
    {
      "id": "particleView",
      "displayDefault": false
    },
    {
      "id": "thermometer",
      "displayDefault": true
    },
    {
      "id": "pH",
      "displayDefault": true
    },
    {
      "id": "vesselTrackingControl",
      "displayDefault": false
    }
  ],
  "transfer": ["precise", "significantFigures","realistic"]
},
            reactions: {
  "REACTIONS": {
    "REACTION": [
      {
        "SPECIES_REF": [
          {
            "id": "0",
            "coefficient": "-1"
          },
          {
            "id": "1",
            "coefficient": "1"
          },
          {
            "id": "2",
            "coefficient": "1"
          }
        ]
      },
	  {
        "SPECIES_REF": [
          {
            "id": 3,
            "coefficient": -1
          },
          {"id": 4,
          "coefficient": -1},
          {"id": 5,
          "coefficient": 1}
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
                  "species": [
                     {
                        "id": 0
                     }
                  ]
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
  const allowLoadAssignment = true;
  const appModel = new VLab.AppModel();
  const appView = new VLab.AppView({ model: appModel, el: document.getElementById("vlab"), vlab: data});
    </script>