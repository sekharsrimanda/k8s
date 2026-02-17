# Interview Notes — Admission Controllers

## Q: When do admission controllers execute?
A: After authN/authZ and before persistence in etcd.

## Q: Validating vs Mutating?
A: Mutating changes objects; Validating approves/denies.

## Q: Why use has() in CEL?
A: Prevent errors when optional fields are missing.

## Q: Admission failure vs Scheduling failure?
A: Admission blocks creation; scheduler runs later.
