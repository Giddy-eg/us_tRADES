# Folder Contents

## 1. notebooks
Contains post-processing files.

## 2. rules
Contains internal files for snakemake, listing the relationships between raw data files in the resources folder, the scripts, and the processed files in the results/data folder.

## 3. Snakefile
A [snakemake](https://snakemake.readthedocs.io/en/stable/) workflow has been implemented to eaisly create new datafiles when iterating through scenarios.

## 3. envs
Contains dependencies for different snakemake rules.

## 5. scripts
Contains python files for processing input data into data ready for being run through otoole, then osemosys.

# Running the Model
To run the model, you will first need to install the [GNU Linear Programming Kit](https://www.gnu.org/software/glpk/). Installation instructions can be found on the [OSeMOSYS repository](https://github.com/OSeMOSYS/OSeMOSYS_GNU_MathProg/tree/master/src#installation). Ensure you follow the optional steps to install [otoole]()https://github.com/OSeMOSYS/otoole and the CBC open-source solver. 

## Dependencies 
### Python Packages 


### Computer Requirements
The base model requires extensive computer resources. 
- 64GB RAM 
- 9hr Run Time using CPLEX 

## How to Run 
1. Navigate to the model folder in the command line 
2. Run the command `conda activate snakemake` to open the snakemake environment (assuming you are using conda)
3. Run the command `snakemake <input_argument> -F -c1` from the command line, replacing `<input_argument>` with  
a.  `data_file` to generate the file holding all data in .txt format  
b.  `lpFile` to generate the file to input into the solver  
c.  `solveCBC` to generate the input datafiles and solve the model using CPLEX  
d. `solveCPLEX` to generate the input datafiles and solve the model using CPLEX

### Solution Quality 
If using CPLEX to solve the model, the following table will print out at the end of the solve. This table presents the quality and stability of the solution. We consider acceptable condition numbers to be on the order of 1e+9 or less, as described on page 151 of the [CPLEX User’s Manual](https://perso.ensta-paris.fr/~diam/ro/online/cplex/cplex1271_pdfs/usrcplex.pdf). We also consider acceptable Ax-b residual numbers to be no larder then the [feasability tolerance](http://www-eio.upc.edu/lceio/manuals/cplex-11/html/refparameterscplex/refparameterscplex47.html) used to solve the model. By default the feasibility tolerance is set to 1e-06 as described on page 272 of the [CPLEX User’s Manual](https://perso.ensta-paris.fr/~diam/ro/online/cplex/cplex1271_pdfs/usrcplex.pdf). 

Max. unscaled (scaled) bound infeas.        =  
Max. unscaled (scaled) reduced-cost infeas. =  
Max. unscaled (scaled) Ax-b resid.          =  
Max. unscaled (scaled) c-B'pi resid.        =  
Max. unscaled (scaled) |x|                  =  
Max. unscaled (scaled) |slack|              =  
Max. unscaled (scaled) |pi|                 =  
Max. unscaled (scaled) |red-cost|           =  
Condition number of scaled basis            =  

## Viewing Results 
The results will be saved to a `.sol` in a `results/` folder under the root directory. If running through CBC, a folder of CSVs will also be created.

### CPLEX Results
Working with the native solution file from CPLEX is difficult. We opt to use a OSeMOSYS Community user created script to convert the CPLEX result file into one that is the same format as CBC. 

### Jupyter Notebooks 
Visualization scripts can be found in `scripts/postPocessing/`. Scripts will read in the `.sol` file 