# How to submit a geometry optimization job of Emim using Gaussion
![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)
## Create the SMILES of Emim
1. Draw a structuer of Emim in ChemDraw.
2. Select the whole structure, right click, then go to "Molecule --> Copy as --> SMILES" and click.
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/bfd6a385-a274-4c83-aac4-b8f6ac862094)
3. Now the SMILES of your selected structure will be copied to your clipboard.
## Convert the SMILES of Emim to 3D structure and optimize the 3D structure based on force field using python
1. Import the python modules which will be used in the next steps. Type the fllowing code in your jupyter notebook and run.
```python3
from rdkit import Chem
from rdkit.Chem import AllChem
```
2. Read the SMILES string using ```Chem.MolFromSmiles```, and write it into a variable named "mol". Use the fllowing code.
```python3
mol = Chem.MolFromSmiles('CN1C=[N+](CC)C=C1')
mol
```
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/d8b3a1ae-3584-4131-b522-2a05c26dcb74)

3. Add hydrogen atoms to "mol".
```python3
mol = Chem.AddHs(mol)
mol
```
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/4e5e671e-2219-4280-8f50-b85bc104ccb8)

4. Convert "mol" to 3D structure and optimize the 3D structure using a universial force field.
```python3
AllChem.EmbedMolecule(mol)
AllChem.UFFOptimizeMolecule(mol)
mol
```
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/1a989832-0131-431f-a031-7617ce6e6dab)

5. Convert "mol" to xyz format and print.
```python3
xyz = Chem.MolToXYZBlock(mol)
print(xyz)
```
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/8497017c-ebf7-4df1-9fc9-2d0e8c1fe455)

These coordinates of the force field optimized 3D structure will be used in the next steps.
## Make a Gaussion input file and submit the optimization job in the cluster
1. Make a new text file in your computer. Then copy the following to the new text file.
```
%mem=3200MB
%nproc=8
% chk=Emim.chk
# B3LYP/6-311+G(d, p) opt freq

Emim

1 1

```
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/662432b0-e770-4b2c-833d-3e5369ed0e94)

2. Copy the corrdinates of the optimized 3D structure generated before to this text file. Please note that copy the corrdinates part only.
   
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/a5591bd7-f70d-43b5-9b87-540ac5ae12b5)
![image](https://github.com/Mengyangjp/tuterial/assets/127812221/4abec8f9-1cd3-4341-a369-8c21bd07c9d7)

3. Connect to the cpu cluster and make an input file.
  - Connect to the cpu cluster using your terminal
  - make a new folder named ```run_Emim_opt``` using the flowing command line
   ```
   mkdir run_Emim_opt
   ```
  - enter the new folder using the following command line
   ```
   cd run_Emim_opt
   ```
  - make a new file named ```Emim_opt.com``` and edit it using vim
```
vi Emim_opt.com
```

  ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/bcd4fef3-f46e-4c80-abc0-f44b1cb4624e)

    
  - copy the content in the new text file made previously
    ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/27c3166b-fc33-4a27-90f9-317151e314e6)
    
  - go back to the terminal connected to cpu cluster
  - type ```i``` to go to the insert mode of vim
    ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/7d8c7c3f-fd85-47a9-8ea8-fd707364ab8c)
    
  - paste the copied content
    ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/6a42e685-c02a-4f70-996a-31a6f4c8c1e9)
    ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/4ca3cd50-7317-4cfa-8154-bde0ddbdf558)
    
  - type ```Esc``` to exit from the insert mode
    ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/ba637182-1490-4195-920a-9dff610782a1)
    
  - type```:wq``` to save the file and quit from vim and press enter
    ![image](https://github.com/Mengyangjp/tuterial/assets/127812221/b7d8f21d-d5f5-4ab8-bb8a-c92a8c0b307a)

## Submit the optimization job
To submit the job using the fllowing command
```
g16 Emim_opt.com
```








