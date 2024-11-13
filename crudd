 public ActionResult AddNewCourses(int? id)
        {
            ViewBag.courses = new SelectList(db.Courses.ToList(), "CourseID", "CourseName", (id != null) ? id.ToString() : "");
            return PartialView("_addNewCourses");
        }
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create(StudentVM studentVM, int[] courseID)
        {
            if (ModelState.IsValid)
            {
                Student student = new Student()
                {
                    StudentName = studentVM.StudentName,
                    Age = studentVM.Age,
                    DateOfBirth = studentVM.DateOfBirth,
                    MorningShift = studentVM.MorningShift,
                };
                HttpPostedFileBase file = studentVM.PictureFile;
                if (file != null)
                {
                    string filePath = Path.Combine("/Images/" + DateTime.Now.Ticks.ToString() + Path.GetExtension(file.FileName));
                    file.SaveAs(Server.MapPath(filePath));
                    student.Picture = filePath;

                }
                foreach (var item in courseID) 
                {
                    Enrollment enrollment = new Enrollment() 
                    {
                        Student = student,
                        StudentID = studentVM.StudentID,
                        CourseID = item,
                    };
                    db.Enrollments.Add(enrollment);
                }
                db.Students.Add(student);
                db.SaveChanges();
                return RedirectToAction("Index");


            }
            return View();
        }
        public ActionResult Edit(int? id)
        {
            Student student = db.Students.First(s => s.StudentID == id);
            var studentVM = new StudentVM()
            {
                StudentID = student.StudentID,
                StudentName = student.StudentName,
                Picture = student.Picture,
                DateOfBirth = student.DateOfBirth,
                MorningShift = student.MorningShift,
                Age = student.Age,
                Enrollments = db.Enrollments.Where(e => e.StudentID == student.StudentID).ToList()
            };
            return View(studentVM);
        }
        [HttpPost]
        public ActionResult Edit(StudentVM studentVM, int[] courseID)
        {
            if (ModelState.IsValid)
            {

                var studObj = db.Students.Find(studentVM.StudentID);
                if (studObj != null)
            {
                    studObj.StudentName = studentVM.StudentName;
                    studObj.Age = studentVM.Age;
                    studObj.MorningShift = studentVM.MorningShift;
                    studObj.DateOfBirth = studentVM.DateOfBirth;
                }
                HttpPostedFileBase file = studentVM.PictureFile;
                if (file != null)
                {
                    string filePath = Path.Combine("/Images/" + DateTime.Now.Ticks.ToString() + Path.GetExtension(file.FileName));
                    file.SaveAs(Server.MapPath(filePath));
                    studObj.Picture = filePath;

                }
                else {
                    studObj.Picture = studObj.Picture;
                }
                var existsCourse = db.Enrollments.Where(s => s.StudentID == studObj.StudentID).ToList();
                foreach (var course in existsCourse)
                {

                    db.Enrollments.Remove(course);
                }
                foreach (var item in courseID) {

                    Enrollment enrollment = new Enrollment()
                    {
                        Student = studObj,
                        StudentID = studentVM.StudentID,
                        CourseID = item,
                    };
                    db.Enrollments.Add(enrollment);
                }
                db.SaveChanges();
                return RedirectToAction("Index");

            }
            return View();
        }

        public ActionResult Delete(int? id) {
            if (id != null) {

                var stdObj = db.Students.FirstOrDefault(s => s.StudentID == id);
                var enrollmentInfo = db.Enrollments.Where(s=>s.StudentID== id).ToList();
                foreach (var enrollment in enrollmentInfo) {
                    db.Enrollments.Remove(enrollment);
                }
                db.Students.Remove(stdObj);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
         return View("Index");
        }
