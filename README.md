# ViLearnX-Task-1
README


    import tkinter as tk
      
    from tkinter import messagebox


    class GradeTrackerApp:
      def __init__(self, root):
        self.root = root
        self.root.title("Student Grade Tracker")
        self.root.geometry("600x450")

        self.subject_entries = []
        self.grades = []

        self.create_widgets()

    def create_widgets(self):
        # Title label
        title_label = tk.Label(self.root, text="Student Grade Tracker", font=("Arial", 16, "bold"))
        title_label.pack(pady=10)

        # Number of subjects entry
        self.subjects_entry = tk.Entry(self.root, width=20)
        self.subjects_entry.pack(pady=5)

        # Submit number of subjects button
        submit_subjects_button = tk.Button(self.root, text="Submit Subjects", command=self.submit_subjects)
        submit_subjects_button.pack(pady=5)

        # Placeholder for subject grades
        self.grades_frame = tk.Frame(self.root)
        self.grades_frame.pack(pady=10)

        # Finish Input button
        self.finish_button = tk.Button(self.root, text="Get Report", command=self.calculate_grades, state=tk.DISABLED)
        self.finish_button.pack(pady=5)

        # Results text
        self.results_text = tk.Text(self.root, height=8, width=50, wrap='word')
        self.results_text.pack(pady=10)
        self.results_text.config(state=tk.DISABLED)

    def submit_subjects(self):
        """Create entry fields for each subject after the number of subjects is entered."""
        self.grades_frame.destroy()  # Remove any previous widgets
        self.grades_frame = tk.Frame(self.root)
        self.grades_frame.pack(pady=10)

        self.grades = []
        num_subjects = self.subjects_entry.get()

        try:
            num_subjects = int(num_subjects)
            if num_subjects <= 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid Input", "Please enter a positive integer for the number of subjects.")
            return

        self.subjects_entry.destroy()
        self.subjects_entry = None

        # Create a label and entry for each subject
        self.subject_entries = []
        for i in range(num_subjects):
            subject_label = tk.Label(self.grades_frame, text=f"Subject {i + 1} (A-F):")
            subject_label.grid(row=i, column=0, padx=5, pady=5)
            subject_entry = tk.Entry(self.grades_frame, width=20)
            subject_entry.grid(row=i, column=1, padx=5, pady=5)
            self.subject_entries.append(subject_entry)

        # Enable finish input button
        self.finish_button.config(state=tk.NORMAL)

    def add_grades(self):
        """Collect grades from the entry fields."""
        self.grades = []
        letter_to_grade = {'A': 10, 'B': 9, 'C': 8, 'D': 7, 'E': 6, 'F': 5}
        for entry in self.subject_entries:
            grade = entry.get().strip().upper()
            if grade in letter_to_grade:
                self.grades.append(letter_to_grade[grade])
            else:
                messagebox.showwarning("Invalid Grade", "Grade must be a letter from A to F.")
                return

    def calculate_grades(self):
        self.add_grades()
        
        if not self.grades:
            messagebox.showwarning("No Grades", "No grades have been entered.")
            return

        average = self.calculate_average(self.grades)
        letter_grade = self.determine_letter_grade(average)
        gpa = self.determine_gpa(average)

        results = (
            f"Number of subjects: {len(self.grades)}\n"
            f"Average grade: {average:.2f}\n"
            f"Letter grade: {letter_grade}\n"
            f"GPA: {gpa:.1f}"
        )

        self.results_text.config(state=tk.NORMAL)
        self.results_text.delete(1.0, tk.END)  # Clear previous results
        self.results_text.insert(tk.END, results)
        self.results_text.config(state=tk.DISABLED)

    def calculate_average(self, grades):
        return sum(grades) / len(grades) if grades else 0

    def determine_letter_grade(self, average):
        # Convert numerical average back to letter grade
        if average >= 9.5:
            return 'A'
        elif average >= 8.5:
            return 'B'
        elif average >= 7.5:
            return 'C'
        elif average >= 6.5:
            return 'D'
        elif average >= 5.5:
            return 'E'
        else:
            return 'F'

    def determine_gpa(self, average):
        return average

    if __name__ == "__main__":
       root = tk.Tk()
       app = GradeTrackerApp(root)
       root.mainloop()
