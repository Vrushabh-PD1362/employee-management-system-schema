Employee Management System : Database Schema Design


Tables

Employees
employee_id BIGINT PK AUTO_INCREMENT
employee_code VARCHAR(40) NOT NULL UNIQUE
name VARCHAR(40) NOT NULL
email  VARCHAR(40) NOT NULL (UNIQUE)
phone VARCHAR(20)
date_of_joining DATE
employee_status ENUM(active, inactive) NOT NULL
employment_type_id INT FK(Employment_Types.employment_type_id)
vendor_id INT NULL FK(Vendors.vendor_id)


Departments
department_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
cost_center VARCHAR(20)
manager_id INT FK(Employees.employee_id)


Employee_Department_History
employee_department_history_id BIGINT PK AUTO_INCREMENT
employee_id INT FK(Employees.employee_id)
department_id INT FK(Departments.department_id)
from_date DATE NOT NULL
to_date DATE NULL
status TINYINT


Reporting_History
reporting_history_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)  
manager_id BIGINT FK(Employees.employee_id)  
from_date DATE NOT NULL
to_date DATE NULL
status TINYINT 


Roles
role_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Employee_Roles
employee_role_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)  
role_id INT FK(Roles.role_id)
from_date DATE NOT NULL
to_date DATE NULL
is_primary_role TINYINT 
status TINYINT


Offices
office_id INT PK AUTO_INCREMENT
address VARCHAR(200) NOT NULL
time_zone VARCHAR(50) 


Employee_Office_History
employee_office_history_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id) 
office_id INT FK(Offices.office_id) 
from_date DATE NOT NULL
to_date DATE NULL
status TINYINT


Compensation_Records
compensation_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)  
basic DECIMAL(10, 2)
hra DECIMAL(10, 2)
allowances DECIMAL(10, 2)
bonuses DECIMAL(10, 2)
gross_total DECIMAL(10, 2)
from_date DATE NOT NULL
to_date DATE NULL
status TINYINT


Work_Types
work_type_id INT PK AUTO_INCREMENT
work_type VARCHAR(40) NOT NULL


Attendances
attendance_id BIGINT PK AUTO_INCREMENT
employee_id  BIGINT FK(Employees.employee_id)  
date DATE
sign_in TIMESTAMP
sign_out TIMESTAMP
work_type_id INT FK(Work_Types.work_type_id)
UNIQUE(employee_id, date)


Leaves
leave_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Leave_Requests
leave_request_id BIGINT PK AUTO_INCREMENT
employee_id  BIGINT FK(Employees.employee_id)  
leave_id INT FK(Leaves.leave_id)
start_date DATE
end_date DATE
approver_id BIGINT FK(Employees.employee_id)  
leave_status ENUM(Approve, Reject, Pending) DEFAULT ‘Pending’


Assets
asset_id INT PK AUTO_INCREMENT
asset_type ENUM(Laptop, Phone)
serial_number VARCHAR(40) NOT NULL UNIQUE


Employee_Assets
employee_asset_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)  
asset_id INT FK(Assets.asset_id)  
issue_date DATE
return_date DATE
status TINYINT


Review_Cycles
review_cycle_id INT PK AUTO_INCREMENT
name VARCHAR(40)
start_date DATE
end_date DATE
Performance_Reviews
performance_review_id INT PK AUTO_INCREMENT
review_cycle_id INT FK(Review_Cycles.review_cycle_id)
employee_id INT FK(Employees.employee_id)
reviewer_id INT FK(Employees.employee_id)
rating DECIMAL(10, 2)
comments VARCHAR(255)
goal TEXT
UNIQUE(review_cycle_id, employee_id)


Certificates
certificate_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Trainings
training_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
certificate_id INT FK(Certificates.certificate_id)


Employee_Trainings
employee_training_id BIGINT PK AUTO_INCREMENT
employee_id  BIGINT FK(Employees.employee_id)
training_id  INT FK(Trainings.training_id)
status ENUM(completed, in_progress, not_started)
completion_date DATE NULL
UNIQUE(employee_id, training_id)


Documents
document_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Employee_Documents
employee_document_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)
document_id INT FK(Documents.document_id)
storage_reference VARCHAR(255)
signed_date DATE


Employee_Separations
separation_id BIGINT PK  AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)
resignation_date DATE
last_working_date DATE
exit_reason VARCHAR(400)
assets_returned TINYINT
fnf_status TINYINT


Access_Control_Groups
access_control_group_id INT PK AUTO_INCREMENT
group_name VARCHAR(40) NOT NULL
permissions VARCHAR(255) 


Employee_Access_Control_Groups
employee_access_control_group_id BIGINT PK AUTO_INCREMENT
employee_id BIGINT FK(Employees.employee_id)
access_control_group_id INT FK(Access_Control_Groups.access_control_group_id)
start_date DATE
end_date DATE
status TINYINT DEFAULT 0


Skills
skill_id INT PK AUTO_INCREMENT
name varchar(40) NOT NULL


Employee_Skills
employee_skill_id BIGINT PK AUTO_INCREMENT
employee_id FK(Employees.employee_id)
skill_id FK(Skills.skill_id)
proficiency_level VARCHAR(40)
years_of_experience DECIMAL(10, 2)
UNIQUE(employee_id, skill_id)


Employment_Types
employment_type_id INT PK AUTO_INCREMENT
name VARCHAR(40)


Vendors
vendor_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL



Cardinality of Relations : 

Between Employees and Employment_Types => m : 1
Between Employees and Vendors =>  m : 1
Between Employees and Departments (current) => m : 1
Between Employees(reporting) and Employees(manager) => m : 1
Between Employees and Offices (current) => m : 1

Between Employees and Compensation_Records => 1 : m
Between Employees and Attendances => 1 : m
Between Work_Types and Attendances => 1 : m
Between Employees and Leave_Requests => 1 : m
Between Leaves and Leave_Requests => 1 : m
Between Employees and Employee_Assets => 1 : m
Between Assets and Employee_Assets => 1 : m
Between Employees and Performance_Reviews => 1 : m
Between Review_Cycles and Performance_Reviews => 1 : m
Between Certificates and Trainings => 1 : m
Between Employees and Employee_Trainings => 1 : m
Between Training and Employee_Trainings => 1 : m
Between Employees and Employee_Documents => 1 : m
Between Documents and Employee_Documents => 1 : m
Between Employees and Employee_Access_Control_Groups => 1 : m
Between Access_Control_Groups and Employee_Access_Control_Groups => 1 : m
Between Employees and Employee_Skills => 1 : m
Between Skills and Employee_Skills => 1 : m

Between Employees and Departments (historical) => m : m
Between Employees and Offices (historical) => m : m
Between Employees and Roles => m : m
Between Employees and Skills => m : m
Between Employees and Trainings => m : m


