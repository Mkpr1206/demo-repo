
import pandas as pd
import os
import random
from datetime import datetime

hospital_name = "GREEN DOLPHIN"
patients_filename = f"{hospital_name}_patients.xlsx"
doctors_filename = f"{hospital_name}_doctors.xlsx"
departments_filename = f"{hospital_name}_departments.xlsx"
specializations_filename = f"{hospital_name}_specializations.xlsx"
MAX_QUEUE_SIZE = 10

def initialize_data():
    """Initialize hospital data if files don't exist."""
    if not os.path.exists(departments_filename):
        departments = ["Cardiology", "Neurology", "Pediatrics", "Orthopedics", "General Medicine"]
        df = pd.DataFrame({"Department Name": departments})
        df.to_excel(departments_filename, index=False)
        print(f"Initialized departments for {hospital_name}.")

    if not os.path.exists(specializations_filename):
        specializations_data = {
            "Department": [
                "Cardiology", "Cardiology", "Cardiology", "Cardiology",
                "Neurology", "Neurology", "Neurology", "Neurology",
                "Pediatrics", "Pediatrics", "Pediatrics", "Pediatrics",
                "Orthopedics", "Orthopedics", "Orthopedics", "Orthopedics",
                "General Medicine", "General Medicine", "General Medicine", "General Medicine"
            ],
            "Specialization": [
                "Heart Surgery", "Vascular Medicine", "Cardiac Imaging", "Interventional Cardiology",
                "Brain Disorders", "Spinal Cord", "Nerve Disorders", "Stroke Management",
                "Neonatal Care", "Adolescent Medicine", "Developmental Disorders", "Pediatric Emergency",
                "Joint Replacement", "Sports Medicine", "Spine Surgery", "Trauma Care",
                "Internal Medicine", "Preventive Care", "Geriatrics", "Family Medicine"
            ]
        }
        spec_df = pd.DataFrame(specializations_data)
        spec_df.to_excel(specializations_filename, index=False)
        print(f"Initialized specializations for {hospital_name}.")

    if not os.path.exists(doctors_filename):
        doctors_data = {
            "Doctor Name": [
                "Dr. Sharma", "Dr. Patel", "Dr. Rodriguez", "Dr. Lee", "Dr. Kim",
                "Dr. Chen", "Dr. Gupta", "Dr. Williams", "Dr. Garcia",
                "Dr. Kumar", "Dr. Johnson", "Dr. Mehta", "Dr. Thompson", "Dr. Lopez", "Dr. Ali",
                "Dr. Martinez", "Dr. Singh", "Dr. Brown", "Dr. Taylor",
                "Dr. Kapoor", "Dr. Wilson", "Dr. Verma", "Dr. Jackson", "Dr. Anderson"
            ],
            "Gender": [
                "Male", "Male", "Male", "Male", "Female",
                "Male", "Male", "Male", "Female",
                "Male", "Male", "Female", "Female", "Male", "Male",
                "Male", "Male", "Male", "Female",
                "Female", "Male", "Female", "Male", "Female"
            ],
            "Age": [
                45, 38, 52, 41, 36,
                41, 49, 36, 44,
                44, 55, 39, 33, 48, 51,
                47, 42, 51, 38,
                37, 46, 40, 52, 43
            ],
            "Department": [
                "Cardiology", "Cardiology", "Cardiology", "Cardiology", "Cardiology",
                "Neurology", "Neurology", "Neurology", "Neurology",
                "Pediatrics", "Pediatrics", "Pediatrics", "Pediatrics", "Pediatrics", "Pediatrics",
                "Orthopedics", "Orthopedics", "Orthopedics", "Orthopedics",
                "General Medicine", "General Medicine", "General Medicine", "General Medicine", "General Medicine"
            ],
            "Specialization": [
                "Heart Surgery", "Vascular Medicine", "Cardiac Imaging", "Interventional Cardiology", "Heart Surgery",
                "Brain Disorders", "Spinal Cord", "Nerve Disorders", "Stroke Management",
                "Neonatal Care", "Adolescent Medicine", "Developmental Disorders", "Pediatric Emergency", "Neonatal Care", "Adolescent Medicine",
                "Joint Replacement", "Sports Medicine", "Spine Surgery", "Trauma Care",
                "Internal Medicine", "Preventive Care", "Geriatrics", "Family Medicine", "Internal Medicine"
            ],
            "Patients In Check-in": [
                "No", "No", "No", "No", "No",
                "No", "No", "No", "No",
                "No", "No", "No", "No", "No", "No",
                "No", "No", "No", "No",
                "No", "No", "No", "No", "No"
            ],
            "Num Patients In Queue": [
                random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9),
                random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9),
                random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9),
                random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9),
                random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9), random.randint(0, 9)
            ]
        }
        df = pd.DataFrame(doctors_data)
        df.to_excel(doctors_filename, index=False)
        print(f"Initialized doctors for {hospital_name}.")

    elif os.path.exists(doctors_filename):
        try:
            doctors_df = pd.read_excel(doctors_filename)


            required_columns = {
                'Gender': 'Not Specified',
                'Age': 0,
                'Patients In Check-in': 'No',
                'Num Patients In Queue': None
            }

            for col, default_val in required_columns.items():
                if col not in doctors_df.columns:
                    if col == 'Num Patients In Queue':
                        doctors_df[col] = [random.randint(0, 9) for _ in range(len(doctors_df))]
                    else:
                        doctors_df[col] = default_val


            doctors_df = doctors_df[doctors_df["Doctor Name"].notna() & (doctors_df["Doctor Name"] != "")]
            doctors_df.to_excel(doctors_filename, index=False)
        except Exception as e:
            print(f"Error updating doctors file: {e}")

def display_patient_data():
    """Display all patient data from Excel file."""
    if not os.path.exists(patients_filename):
        print(f"No patient data found for {hospital_name}.")
        return

    try:
        df = pd.read_excel(patients_filename)
        if df.empty:
            print(f"No patient data found for {hospital_name}.")
        else:
            print(f"Patient data for {hospital_name}:")
            print(df)
    except pd.errors.EmptyDataError:
        print(f"The patient file exists but contains no data.")
    except Exception as e:
        print(f"Error reading patient data: {e}")

def ensure_doctor_columns(doctors_df):
    """Ensure all necessary columns exist in the doctors dataframe."""
    if 'Gender' not in doctors_df.columns:
        doctors_df['Gender'] = 'Not Specified'
    if 'Age' not in doctors_df.columns:
        doctors_df['Age'] = 0
    if 'Patients In Check-in' not in doctors_df.columns:
        doctors_df['Patients In Check-in'] = 'No'
    if 'Num Patients In Queue' not in doctors_df.columns:
        doctors_df['Num Patients In Queue'] = [random.randint(0, 9) for _ in range(len(doctors_df))]


    return doctors_df[doctors_df["Doctor Name"].notna() & (doctors_df["Doctor Name"] != "")]

def view_departments_and_doctors():
    """View departments and doctors by department."""
    if not os.path.exists(departments_filename) or not os.path.exists(doctors_filename):
        print("Department or doctor data not initialized. Please restart the program.")
        return None

    try:
        departments_df = pd.read_excel(departments_filename)
        doctors_df = pd.read_excel(doctors_filename)
        doctors_df = ensure_doctor_columns(doctors_df)

        print("\nDepartments:")
        for i, dept in enumerate(departments_df["Department Name"], 1):
            print(f"{i}. {dept}")

        try:
            dept_choice = int(input("\nSelect department number to view doctors (0 to return): "))
            if dept_choice == 0:
                return None

            if 1 <= dept_choice <= len(departments_df):
                selected_department = departments_df["Department Name"].iloc[dept_choice-1]
            else:
                print("Invalid department number.")
                return None
        except ValueError:
            print("Invalid input.")
            return None

        dept_doctors = doctors_df[doctors_df["Department"] == selected_department]

        if dept_doctors.empty:
            print(f"No doctors found in {selected_department} department.")
            return None

        print(f"\nDoctors in {selected_department} department:")
        print(f"{'No.':<4}{'Name':<20}{'Gender':<10}{'Age':<6}{'Specialization':<25}{'Status':<15}{'Check-in':<10}{'Queue':<6}")
        print("-" * 100)

        for i, (_, doctor) in enumerate(dept_doctors.iterrows(), 1):
            name = doctor["Doctor Name"]
            gender = doctor["Gender"]
            age = doctor["Age"]
            spec = doctor["Specialization"]
            checkin = doctor["Patients In Check-in"]
            queue = doctor["Num Patients In Queue"]


            if queue == 0 and checkin == "No":
                status = "AVAILABLE"
            else:
                status = "NOT AVAILABLE"

            print(f"{i:<4}{name:<20}{gender:<10}{age:<6}{spec:<25}{status:<15}{checkin:<10}{queue:<6}")

        max_doctors_per_dept = 6
        empty_spots = max_doctors_per_dept - len(dept_doctors)
        if empty_spots > 0:
            print(f"\n{empty_spots} open position(s) available in this department")

        input("\nPress Enter to continue...")
    except pd.errors.EmptyDataError:
        print("One of the data files is empty. Please reinitialize the system.")
    except Exception as e:
        print(f"Error viewing departments and doctors: {e}")

def remove_doctor():
    """Remove a doctor and reassign their patients."""
    if not os.path.exists(doctors_filename):
        print("No doctor data found. Please initialize the system first.")
        return

    try:
        doctors_df = pd.read_excel(doctors_filename)
        if doctors_df.empty:
            print("No doctors found in the database.")
            return

        doctors_df = ensure_doctor_columns(doctors_df)
        departments = doctors_df["Department"].unique()
        print("\nDepartments:")
        for i, dept in enumerate(departments, 1):
            print(f"{i}. {dept}")

        dept_choice = input("\nSelect department number (or press Enter to show all doctors): ")

        if dept_choice.strip():
            try:
                dept_idx = int(dept_choice) - 1
                if 0 <= dept_idx < len(departments):
                    selected_dept = departments[dept_idx]
                    filtered_doctors = doctors_df[doctors_df["Department"] == selected_dept]
                else:
                    print("Invalid department selection.")
                    return
            except ValueError:
                print("Invalid input.")
                return
        else:
            filtered_doctors = doctors_df

        if filtered_doctors.empty:
            print("No doctors found with the selected criteria.")
            return

        print("\nDoctors:")
        print(f"{'No.':<4}{'Name':<20}{'Gender':<10}{'Age':<6}{'Department':<20}{'Specialization':<25}{'Check-in':<10}{'Queue':<6}")
        print("-" * 100)

        for i, (_, doctor) in enumerate(filtered_doctors.iterrows(), 1):
            name = doctor["Doctor Name"]
            gender = doctor["Gender"]
            age = doctor["Age"]
            department = doctor["Department"]
            spec = doctor["Specialization"]
            checkin = doctor["Patients In Check-in"]
            queue = doctor["Num Patients In Queue"]

            print(f"{i:<4}{name:<20}{gender:<10}{age:<6}{department:<20}{spec:<25}{checkin:<10}{queue:<6}")

        try:
            doctor_choice = int(input("\nEnter the number of the doctor to remove (0 to cancel): "))
            if doctor_choice == 0:
                return

            if 1 <= doctor_choice <= len(filtered_doctors):
                selected_doctor = filtered_doctors.iloc[doctor_choice-1]["Doctor Name"]
                department = filtered_doctors.iloc[doctor_choice-1]["Department"]

                # Check for assigned patients
                if os.path.exists(patients_filename):
                    patients_df = pd.read_excel(patients_filename)
                    if "Assigned Doctor" in patients_df.columns:
                        assigned_patients = patients_df[patients_df["Assigned Doctor"] == selected_doctor]
                        if not assigned_patients.empty:
                            print(f"\nWarning: {selected_doctor} has {len(assigned_patients)} assigned patients.")
                            confirm = input("Do you want to proceed with removal? (y/n): ").lower()
                            if confirm != 'y':
                                return

                            # Update patient records
                            patients_df.loc[patients_df["Assigned Doctor"] == selected_doctor, "Assigned Doctor"] = "Not Assigned"
                            patients_df.to_excel(patients_filename, index=False)
                            print(f"Updated {len(assigned_patients)} patient records.")

                # Remove doctor record
                doctors_df = doctors_df[doctors_df["Doctor Name"] != selected_doctor]
                doctors_df.to_excel(doctors_filename, index=False)
                print(f"\n{selected_doctor} has been removed from {department} department.")
            else:
                print("Invalid selection.")
        except ValueError:
           print("Invalid input.")
    except Exception as e:
        print(f"Error removing doctor: {e}")

def admissions_menu():
    """Display and handle the admissions menu."""
    while True:
        print("\nAdmissions Menu:")
        print("1. Patient Admission (General)")
        print("2. Patient Admission (Emergency)")
        print("3. Add New Doctor")
        print("4. Remove Doctor")
        print("5. Return to Main Menu")
        choice = input("Enter your choice: ")

        if choice == "1":
            add_patient_data("General")
        elif choice == "2":
            add_patient_data("Emergency")
        elif choice == "3":
            add_doctor_data()
        elif choice == "4":
            remove_doctor()
        elif choice == "5":
            return
        else:
            print("Invalid choice! Try again.")

def get_specializations_for_department(department):
    """Get available specializations for a given department."""
    try:
        if not os.path.exists(specializations_filename):
            return ["General"]

        spec_df = pd.read_excel(specializations_filename)
        dept_specs = spec_df[spec_df["Department"] == department]["Specialization"].tolist()

        if not dept_specs:
            return ["General"]

        return dept_specs
    except pd.errors.EmptyDataError:
        print("Specializations file is empty.")
        return ["General"]
    except Exception as e:
        print(f"Error getting specializations: {e}")
        return ["General"]

def add_patient_data(admission_type):
    """Add a new patient to the system."""
    patient_name = input("Enter patient name: ")
    if not patient_name.strip():
        print("Patient name cannot be empty.")
        return

    while True:
        gender = input("Enter patient gender (Male/Female/Other): ").capitalize()
        if gender in ["Male", "Female", "Other"]:
            break
        print("Invalid input. Please enter Male, Female, or Other.")

    while True:
        try:
            patient_age = int(input("Enter patient age: "))
            if patient_age > 0:
                break
            print("Age must be a positive number.")
        except ValueError:
            print("Invalid input. Please enter a number.")

    try:
        # Select department
        if os.path.exists(departments_filename):
            departments_df = pd.read_excel(departments_filename)
            print("\nAvailable Departments:")
            for i, dept in enumerate(departments_df["Department Name"], 1):
                print(f"{i}. {dept}")

            while True:
                try:
                    dept_choice = int(input("Select department number: "))
                    if 1 <= dept_choice <= len(departments_df):
                        department = departments_df["Department Name"].iloc[dept_choice-1]
                        break
                    print("Invalid choice. Please select a valid department number.")
                except ValueError:
                    print("Invalid input. Please enter a number.")
        else:
            department = "General Medicine"

        doctor_name = "Not Assigned"

        # Show available doctors
        if os.path.exists(doctors_filename):
            doctors_df = pd.read_excel(doctors_filename)
            doctors_df = ensure_doctor_columns(doctors_df)

            dept_doctors = doctors_df[doctors_df["Department"] == department]

            if not dept_doctors.empty:
                print(f"\nDoctors in {department} department:")
                print(f"{'No.':<4}{'Name':<20}{'Gender':<10}{'Age':<6}{'Specialization':<25}{'Status':<15}{'Check-in':<10}{'Queue':<6}")
                print("-" * 100)

                available_doctors = []

                for i, (idx, doctor) in enumerate(dept_doctors.iterrows(), 1):
                    name = doctor["Doctor Name"]
                    gender = doctor["Gender"]
                    age = doctor["Age"]
                    spec = doctor["Specialization"]
                    checkin = doctor["Patients In Check-in"]
                    queue = doctor["Num Patients In Queue"]

                    # Updated availability logic
                    if queue == 0 and checkin == "No":
                        status = "AVAILABLE"
                        available = True
                    else:
                        status = "NOT AVAILABLE"
                        available = False

                    print(f"{i:<4}{name:<20}{gender:<10}{age:<6}{spec:<25}{status:<15}{checkin:<10}{queue:<6}")

                    if available:
                        available_doctors.append((i, name))

                if available_doctors or admission_type == "Emergency":
                    while True:
                        try:
                            doc_choice = int(input("\nSelect doctor number (0 for none): "))
                            if doc_choice == 0:
                                doctor_name = "Not Assigned"
                                break

                            if 1 <= doc_choice <= len(dept_doctors):
                                selected_doctor = dept_doctors.iloc[doc_choice-1]["Doctor Name"]
                                doctor_index = dept_doctors.iloc[doc_choice-1].name

                                # Check queue limit
                                current_queue = doctors_df.at[doctor_index, "Num Patients In Queue"]
                                current_checkin = doctors_df.at[doctor_index, "Patients In Check-in"]

                                # Check if doctor is truly available (queue=0 and not with patient)
                                is_available = (current_queue == 0 and current_checkin == "No")

                                if not is_available and admission_type != "Emergency":
                                    print(f"Warning: {selected_doctor} is not available.")
                                    confirm = input("This is not an emergency. Assign anyway? (y/n): ").lower()
                                    if confirm != 'y':
                                        continue

                                doctor_name = selected_doctor

                                # Update the doctor's patient queue count
                                doctors_df.at[doctor_index, "Num Patients In Queue"] += 1
                                doctors_df.to_excel(doctors_filename, index=False)
                                break
                            else:
                                print("Invalid choice. Please select a valid doctor number.")
                        except ValueError:
                            print("Invalid input. Please enter a number.")
                else:
                    print(f"No available doctors in {department} department.")
            else:
                print(f"No doctors found in {department} department.")

        # Get current date
        current_date = datetime.now().strftime("%Y-%m-%d")

        # Create new patient data
        new_data = pd.DataFrame({
            "Patient Name": [patient_name],
            "Gender": [gender],
            "Age": [patient_age],
            "Department": [department],
            "Assigned Doctor": [doctor_name],
            "Admission Type": [admission_type],
            "Admission Date": [current_date]
        })

        # Add to existing file or create new one
        if os.path.exists(patients_filename):
            try:
                df = pd.read_excel(patients_filename)

                # Ensure columns exist
                for col in new_data.columns:
                    if col not in df.columns:
                        df[col] = "Not Specified" if col == "Gender" else (0 if col == "Age" else "N/A")

                df = pd.concat([df, new_data], ignore_index=True)
            except Exception as e:
                print(f"Error reading existing patient file. Creating new file. Error: {e}")
                df = new_data
        else:
            df = new_data

        df.to_excel(patients_filename, index=False)
        print(f"Patient admitted to {hospital_name} successfully.")

    except Exception as e:
        print(f"Error adding patient data: {e}")

def update_doctor_checkin_status():
    """Update doctor check-in status and adjust patient queue."""
    if not os.path.exists(doctors_filename):
        print("No doctor data found. Please initialize the system first.")
        return

    try:
        doctors_df = pd.read_excel(doctors_filename)
        if doctors_df.empty:
            print("No doctors found in the database.")
            return

        doctors_df = ensure_doctor_columns(doctors_df)

        print("\nUpdate Doctor Check-in Status:")
        print(f"{'No.':<4}{'Name':<20}{'Department':<20}{'Status':<10}{'Queue':<6}")
        print("-" * 60)

        for i, (_, doctor) in enumerate(doctors_df.iterrows(), 1):
            name = doctor["Doctor Name"]
            department = doctor["Department"]
            checkin = doctor["Patients In Check-in"]
            queue = doctor["Num Patients In Queue"]

            print(f"{i:<4}{name:<20}{department:<20}{checkin:<10}{queue:<6}")

        try:
          doctor_choice = int(input("\nEnter the number of the doctor to update (0 to cancel): "))
            if doctor_choice == 0:
                return

            if 1 <= doctor_choice <= len(doctors_df):
                doctor_index = doctors_df.index[doctor_choice - 1]
                doctor_name = doctors_df.loc[doctor_index, "Doctor Name"]
                current_status = doctors_df.loc[doctor_index, "Patients In Check-in"]
                current_queue = doctors_df.loc[doctor_index, "Num Patients In Queue"]

                print(f"Current check-in status for {doctor_name}: {current_status}")
                new_status = "No" if current_status == "Yes" else "Yes"

                confirm = input(f"Change status to {new_status}? (y/n): ").lower()
                if confirm == 'y':
                    doctors_df.at[doctor_index, "Patients In Check-in"] = new_status


                    if new_status == "Yes" and current_queue > 0:
                        doctors_df.at[doctor_index, "Num Patients In Queue"] = current_queue - 1

                    doctors_df.to_excel(doctors_filename, index=False)

                    if new_status == "Yes" and current_queue > 0:
                        print(f"Updated {doctor_name}'s check-in status to {new_status} and decreased queue to {current_queue - 1}.")
                    else:
                        print(f"Updated {doctor_name}'s check-in status to {new_status}.")
                else:
                    print("Operation cancelled.")
            else:
                print("Invalid selection.")
        except ValueError:
            print("Invalid input.")
    except Exception as e:
        print(f"Error updating doctor check-in status: {e}")

def add_doctor_data():
    """Add a new doctor to the system."""
    try:
        doctor_name = input("Enter doctor name: ")
        if not doctor_name.strip():
            print("Doctor name cannot be empty.")
            return


        if os.path.exists(doctors_filename):
            doctors_df = pd.read_excel(doctors_filename)
            if "Doctor Name" in doctors_df.columns and doctor_name in doctors_df["Doctor Name"].values:
                print(f"Error: Doctor '{doctor_name}' already exists. Please use a different name.")
                return


        while True:
            gender = input("Enter doctor gender (Male/Female/Other): ").capitalize()
            if gender in ["Male", "Female", "Other"]:
                break
            print("Invalid input. Please enter Male, Female, or Other.")


        while True:
            try:
                age = int(input("Enter doctor age: "))
                if 25 <= age <= 75:
                    break
                print("Age must be between 25 and 75 for doctors.")
            except ValueError:
                print("Invalid input. Please enter a number.")


        if os.path.exists(departments_filename):
            departments_df = pd.read_excel(departments_filename)
            print("\nAvailable Departments:")
            for i, dept in enumerate(departments_df["Department Name"], 1):
                print(f"{i}. {dept}")

            while True:
                try:
                    dept_choice = int(input("Select department number: "))
                    if 1 <= dept_choice <= len(departments_df):
                        department = departments_df["Department Name"].iloc[dept_choice-1]
                        break
                    print("Invalid choice. Please select a valid department number.")
                except ValueError:
                    print("Invalid input. Please enter a number.")
        else:
            department = input("Enter department: ")


        specializations = get_specializations_for_department(department)

        if specializations:
            print("\nAvailable Specializations:")
            for i, spec in enumerate(specializations, 1):
                print(f"{i}. {spec}")

            while True:
                try:
                    spec_choice = int(input("Select specialization number: "))
                    if 1 <= spec_choice <= len(specializations):
                        specialization = specializations[spec_choice-1]
                        break
                    print("Invalid choice. Please select a valid specialization number.")
                except ValueError:
                    print("Invalid input. Please enter a number.")
        else:
            specialization = input("Enter specialization: ")


        initial_queue = random.randint(0, 9)


        new_data = pd.DataFrame({
            "Doctor Name": [doctor_name],
            "Gender": [gender],
            "Age": [age],
            "Department": [department],
            "Specialization": [specialization],
            "Patients In Check-in": ["No"],
            "Num Patients In Queue": [initial_queue]
        })


        if os.path.exists(doctors_filename):
            try:
                df = pd.read_excel(doctors_filename)
                df = ensure_doctor_columns(df)
                df = pd.concat([df, new_data], ignore_index=True)
            except Exception as e:
                print(f"Error reading existing doctor file. Creating new file. Error: {e}")
                df = new_data
        else:
            df = new_data

        df.to_excel(doctors_filename, index=False)
        print(f"Doctor data added to {hospital_name} successfully.")

    except Exception as e:
        print(f"Error adding doctor data: {e}")

def main():
    """Main program function."""
    print(f"WELCOME TO {hospital_name}")
    current_date = datetime.now().strftime("%B %d, %Y")
    print(f"Today's Date: {current_date}")

    try:
        initialize_data()

        while True:
            print("\nMain Menu:")
            print("1. View Patient Data")
            print("2. View Departments and Doctors")
            print("3. Admissions")
            print("4. Check Doctor Availability")
            print("5. Exit")

            choice = input("Enter your choice: ")

            if choice == "1":
                display_patient_data()
            elif choice == "2":
                view_departments_and_doctors()
            elif choice == "3":
                admissions_menu()
            elif choice == "4":
                update_doctor_checkin_status()
            elif choice == "5":
                print("Exiting...")
                break
            else:
                print("Invalid choice! Try again.")
    except Exception as e:
        print(f"An error occurred: {e}")
        print("Exiting the program due to an error.")

if __name__ == "__main__":
    main()
