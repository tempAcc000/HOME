# Heard-Of Modeling Language (HOML)
## HOML translator
```
Usage: translator [options] infile outfile
Options:
  -n <num> -- Set the number of processes to <num>.
  -a       -- Generate a formula to verify Agreement.
  -u       -- Generate a formula to verify Univalence.
  -i       -- Generate a formula to verify Invariant.
  -s       -- Generate smt2 code to verify.(default)
  -b       -- Generate smt2 bv code to verify.
```
## verification
To verify the agreementof a consensus protocol, you need to verify the Invariant, Univalence and Agreement . If the verification result of SAT/SMT is UNSAT, the algorithm satisfies this property; otherwise, the algorithm does not meet this property. If a property is not satisfied, you can add (get-model) to the corresponding SMT file and use the SMT solver to get a set of counter examples.
### SMT
Use the -s option during translation to generate SMT files in the format of SMT-lib2.6, and use SMT solver to solve the generated SMT files. You can solve the SMT files using SMT solvers such as Z3 , CVC4.
### SAT
Use the -b option during translation to generate SMT files that use QF_BV LOGIC. Then you can use tools such as yices to convert SMT questions into SAT questions in DIMACS format.
## example
```
./translator -s -i -n 3 ./LastVoting.ho ./invariant3.smt2 
./translator -s -u -n 3 ./LastVoting.ho ./univalence3.smt2 
./translator -s -a -n 3 ./LastVoting.ho ./agreement3.smt2 
z3 ./invariant3.smt2
z3 ./univalence3.smt2 
z3 ./agreement3.smt2 
```
If all three verification results of z3 are UNSAT, then LastVoting algorithm satisfies Agreement  in the case of 3 processes.
# Heard-Of Modeling Environment (HOME)
## Environment
Windows 10/11
Windows Subsystem for Linux (WSL)
Eclipse
## Setup
- Install Eclipse on Windows https://www.eclipse.org/
- Install Xtext in Eclipse：Help-Eclipse Marketplace , search Xtext, install
- Copy the plugin folder to the Eclipse installation directory and restart Eclipse. If the plug-in doesn't work, you can delete the eclipse/configuration/org. The eclipse update folder and restart eclipse.
- Install Windows Subsystem for Linux https://learn.microsoft.com/en-us/windows/wsl/install
- Install SAT/SMT solvers such as z3, CVC4, Yices, Alt-Ergo, minisat, Glucose, Glucose-syrup in wsl . We recommend using apt to install these solvers.
- Use soft link in the wsl to link or copy a translator to /bin

z3：https://github.com/Z3Prover/z3
yices-smt2：https://yices.csl.sri.com/
cvc4：https://cvc4.github.io/downloads.html
Alt-ergo：https://alt-ergo.ocamlpro.com/#releases
Glucose和Glucose-syrup：https://www.labri.fr/perso/lsimon/glucose
minisat：http://minisat.se/
## Usage
- Create a new file in Eclipse with the suffix "ho" . When you edit the HOML code in the file, the code editor will highlight the keywords and automatically check for some simple syntax errors
- Click configuration on the toolbar, select the HOML file path in the upper part, and set the path for saving the file. In the middle of the configuration bar you can configure the translator, select the number of nodes to validate, and by nature, translate to SAT/SMT (Translating to SAT requires yices installed in wsl). Under the configuration bar, you can select the SAT/SMT solver you want to use. The SAT solver supports parallel solving.
- Click the Translate button in the toolbar, and HOME will call the translator to translate the HOML file into the SAT/SMT file
- Click the solve button in the toolbar and HOME will call the SAT/SMT solver in the wsl based on the configuration and output the solution to the selected folder.