import datetime
import json

class Course:
    def __init__(self, code, name, teacher_name, email, credits, lecture_time_location, assignments=None, midterms=None, finals=None, notes="", deadlines=None):
       self.code = code
       self.name = name
       self.teacher_name = teacher_name
       self.email = email
       self.credits = credits
       self.lecture_time_location = lecture_time_location
       self.assignments = assignments if assignments is not None else []
       self.midterms = midterms if midterms is not None else []
       self.finals = finals if finals is not None else []
       self.notes = notes
       self.deadlines = deadlines if deadlines is not None else {}

    def __str__(self):
        return f"Code: {self.code}, Name: {self.name}, Teacher Name: {self.teacher_name}, Email: {self.email}, Credits: {self.credits}, Lecture Time Location: {self.lecture_time_location}"

    def add_assignment(self, assignment):
        self.assignments.append(assignment)

    def add_midterm(self, midterm):
        self.midterms.append(midterm)

    def add_final(self, final):
        self.finals.append(final)

    def add_note(self, note):
        self.notes += note + "\n"

    def add_deadline(self, deadline, description):
        self.deadlines[deadline.strftime('%Y-%m-%d')] = description

    def most_urgent_deadline(self):
        if not self.deadlines:
            return "No deadlines set for this course."
        else:
            return min(self.deadlines.keys())

    def to_dict(self):
        return {
            "code": self.code,
            "name": self.name,
            "teacher_name": self.teacher_name,
            "email": self.email,
            "credits": self.credits,
            "lecture_time_location": self.lecture_time_location,
            "assignments": self.assignments,
            "midterms": self.midterms,
            "finals": self.finals,
            "notes": self.notes,
            "deadlines": self.deadlines
        }

    def save_to_file(self, file_name):
        with open(file_name, 'w') as file:
            json.dump(self.to_dict(), file)

    @classmethod
    def load_from_file(cls, file_name):
        with open(file_name, 'r') as file:
            data = json.load(file)
        course = cls(**data)
        return course

course_A = Course("ECC108", "OBJECT-ORIENTED PROGRAMMING", "Amr ABDELBARI", "amr.abdelbari@neu.edu.tr", 3, "Thursday 15:00 AM - VT 2 D05")

course_A.add_assignment("Assignment 1 - Due March 22")
course_A.add_midterm("Midterm Exam - April 1")
course_A.add_final("Final Exam - May 20")

course_A.add_note("Practice coding regularly.")

course_A.add_deadline(datetime.date(2024, 3, 22), "Assignment 1 due")
course_A.add_deadline(datetime.date(2024, 4, 1), "Midterm Exam")
course_A.add_deadline(datetime.date(2024, 5, 20), "Final Exam")

print(f"Most urgent deadline for Course A: {course_A.most_urgent_deadline()}")
print(course_A)

course_A.save_to_file("course_A.json")

course_A_loaded = Course.load_from_file("course_A.json")
print("\nLoaded Course:")
print(course_A_loaded)

