import os
import shutil
import subprocess

def modify_propagate_param(file_path):
    # Read the original content of the file
    with open(file_path, 'r') as file:
        content = file.readlines()

    # Ask the user for the electric field value
    electric_field = input("Please enter the electric field value: ")

    # Modify the max-electric-field parameter
    for i, line in enumerate(content):
        if line.strip().startswith("max-electric-field double"):
            content[i] = f"max-electric-field double {electric_field}\n"
            break

    # Ask the user for the omega value
    omega_value = input("Please enter the omega value: ")

    # Modify the omega parameter
    for i, line in enumerate(content):
        if line.strip().startswith("omega double"):
            content[i] = f"omega double {omega_value}\n"
            break

    # Write the modified content back to the file
    with open(file_path, 'w') as file:
        file.writelines(content)

    print(f"The file '{file_path}' has been successfully updated.")

def create_and_copy(calculation_name):
    # Paths
    base_path = "/wsu/home/gw/gw45/gw4592/argonattocal"
    source_path = "/wsu/home/gw/gw45/gw4592/main/qprop3_2"
    original_propagate_param = os.path.join(source_path, "attoclock", "propagate.param")
    new_folder_path = os.path.join(base_path, calculation_name)
    new_qprop_path = os.path.join(new_folder_path, "qprop3_2")
    attoclock_path = os.path.join(new_qprop_path, "attoclock")
    script_path = os.path.join(attoclock_path, "do_all.sh")

    # Modify propagate.param in original location
    modify_propagate_param(original_propagate_param)

    # Create new folder
    try:
        os.makedirs(new_folder_path)
        print(f"Created new folder: {new_folder_path}")
    except OSError as error:
        print(f"Error creating folder {new_folder_path}: {error}")
        return False

    # Copy qprop3_2 folder to new location
    try:
        shutil.copytree(source_path, new_qprop_path)
        print(f"Copied {source_path} to {new_qprop_path}")
    except shutil.Error as error:
        print(f"Error copying folder: {error}")
        return False

    # Change permissions of do_all.sh to 755
    try:
        subprocess.run(["chmod", "755", script_path], check=True)
        print(f"Changed permissions of {script_path} to 755")
    except subprocess.CalledProcessError as error:
        print(f"Error changing file permissions: {error}")
        return False

    return True

def modify_slurm_file(file_path, job_name):
    # Read the original content of the file
    with open(file_path, 'r') as file:
        content = file.readlines()

    # Get the current working directory
    current_directory = os.getcwd()

    # Modify the --job-name parameter
    for i, line in enumerate(content):
        if line.startswith("#SBATCH --job-name"):
            content[i] = f"#SBATCH --job-name {job_name}\n"
            break

    # Insert the current directory into line 11 after "cd"
    for i, line in enumerate(content):
        if line.startswith("cd "):
            content[i] = f"cd {current_directory}\n"
            break

    # Insert the current directory into line 17 and append "/do_all.sh"
    for i, line in enumerate(content):
        if line.strip().endswith("do_all.sh"):
            content[i] = f"{current_directory}/do_all.sh\n"
            break

    # Write the modified content back to the file
    with open(file_path, 'w') as file:
        file.writelines(content)

    print(f"The file '{file_path}' has been successfully updated.")

    # Execute the subfile using sbatch
    result = subprocess.run(["sbatch", file_path], capture_output=True, text=True)

    # Print the result of the sbatch command
    if result.returncode == 0:
        print(f"Job submitted successfully. Output:\n{result.stdout}")
    else:
        print(f"Failed to submit job. Error:\n{result.stderr}")

if __name__ == "__main__":
    # Prompt for calculation name
    calculation_name = input("Please enter the calculation name: ")

    # Create and copy folders
    if create_and_copy(calculation_name):
        # Change directory to attoclock folder
        attoclock_path = os.path.join("/wsu/home/gw/gw45/gw4592/argonattocal", calculation_name, "qprop3_2", "attoclock")
        os.chdir(attoclock_path)

        # Modify slurm file
        modify_slurm_file("subfile", calculation_name)
