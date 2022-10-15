class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}
    
    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in lecturer.courses_attached and course in self.courses_in_progress:
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return 'Ошибка'

    def middle_grade(self):
        self.mid_grade = sum(sum(self.grades.values(),[]))/len(sum(self.grades.values(),[]))
        return self.mid_grade

    def __str__(self):
        return (f'Имя:  {self.name }\n'
        f'Фамилия: {self.surname}\n'f'Средняя оценка за домашние задания: {self.middle_grade()}\n'
        f'Курсы в процессе изучения: {",".join(self.courses_in_progress)}\n'
        f'Завершенные курсы: {",".join(self.finished_courses)}')

    def __lt__(self, other):
        if not isinstance(other, Student):
            print('Нельзя сравнить!')
            return
        return self.mid_grade < other.mid_grade    


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

class Lecturer (Mentor):
    def __init__(self, name, surname):
       super().__init__(name, surname)
       self.grades = {}

    def middle_grade(self):
        self.mid_grade = sum(sum(self.grades.values(),[]))/len(sum(self.grades.values(),[]))
        return self.mid_grade

    def __str__(self):
            res = f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {self.middle_grade()}'
            return res    
        
    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            print('Нельзя сравнить!')
            return
        return self.mid_grade < other.mid_grade

class Reviewer (Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        res = f'Имя: {self.name}\nФамилия: {self.surname}'
        return res  

some_student = Student('Ruoy', 'Eman', 'your_gender')
some1_student = Student('Cat', 'Black', 'M')
some_student.courses_in_progress += ['Python']
some_student.finished_courses += ['Введение в программирование']
some1_student.courses_in_progress += ['Python']
some1_student.finished_courses += ['Введение в программирование', 'Git']

some_lecturer = Lecturer('Some', 'Buddy')
some1_lecturer = Lecturer('Sleep', 'Green')
some_lecturer.courses_attached += ['Python']
some1_lecturer.courses_attached += ['Python', 'Git']

some_reviewer = Reviewer('Some', 'Buddy')
some1_reviewer = Reviewer('Green', 'Sleep')
some_reviewer.courses_attached += ['Python']
some1_reviewer.courses_attached += ['Python', 'Git']

some_student.rate_lecturer(some_lecturer, 'Python', 9)
some1_student.rate_lecturer(some_lecturer, 'Python', 8)
some_student.rate_lecturer(some1_lecturer, 'Python', 10 )
some_student.rate_lecturer(some1_lecturer, 'Python', 9 )
some1_student.rate_lecturer(some1_lecturer, 'Python', 10)

some_reviewer.rate_hw(some_student, 'Python', 8)
some1_reviewer.rate_hw(some_student, 'Python', 9)
some_reviewer.rate_hw(some1_student, 'Python', 10)
some_reviewer.rate_hw(some1_student, 'Python', 9)
some1_reviewer.rate_hw(some1_student, 'Python', 10)

some_student.middle_grade()
some1_student.middle_grade()
print(some_student < some1_student)
print(some_student)
print(some1_student)

some_lecturer.middle_grade()
some1_lecturer.middle_grade()
print(some_lecturer < some1_lecturer)
print(some_lecturer)
print(some1_lecturer)

print(some_reviewer)
print(some1_reviewer)

students_list = [some_student, some1_student]
def grade_student(students_list, course):
    sum = 0
    count = 0
    for x in students_list:
        for y in x.grades[course]:
            sum += y
            count += 1
    return round(sum/count, 1)

lecturers_list = [some_lecturer, some1_lecturer]
def grade_lecturer(lecturers_list, course):
    sum = 0
    count = 0
    for x in lecturers_list:
        for y in x.grades[course]:
            sum += y
            count += 1
    return round(sum/count, 1)

print(grade_student(students_list, 'Python'))
print(grade_lecturer(lecturers_list, 'Python'))