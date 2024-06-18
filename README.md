Attoclock Automation Script

This script automates the process of modifying specific parameters in the propagate.param file, creating a new calculation directory, copying necessary files, and submitting a SLURM job for attoclock calculations.

Pre-requisites

Load Required Modules
Before running the script, ensure you have the required modules loaded:
module load gsl
module load gnu7

Create a folder named "argonattocal" in your home directory
Copy the "main" folder into your home directory

Edit the script to update the paths on lines 36, 37, and 121 to match your access ID. Change gw/gw45/gw4592 to your specific access ID.

Running the Script
python attoclockauto.py

User Prompts
When running the script, you will be prompted to enter the following information:
Electric Field Value:

Enter the desired electric field value to replace the current value in the propagate.param file.
Omega Value:

Enter the desired omega value to replace the current value in the propagate.param file.
Calculation Name:

Enter the name of the calculation. This will be used to create a new folder and as the job name for SLURM submission.
