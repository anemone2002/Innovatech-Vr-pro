import React from 'react';
import { useFormik } from 'formik';
import * as Yup from 'yup';
import axios from 'axios';
import './RegistrationForm.css';

const RegistrationForm = () => {
  const formik = useFormik({
    initialValues: {
      name: '',
      email: '',
      password: '',
      confirmPassword: ''
    },
    validationSchema: Yup.object({
      name: Yup.string().required('Required'),
      email: Yup.string().email('Invalid email address').required('Required'),
      password: Yup.string().min(8, 'Password must be 8 characters or more').required('Required'),
      confirmPassword: Yup.string().oneOf([Yup.ref('password'), null], 'Passwords must match').required('Required')
    }),
    onSubmit: values => {
      axios.post('/api/register', values)
        .then(response => alert('Registration successful'))
        .catch(error => alert('Registration failed'));
    }
  });

  return (
    <form onSubmit={formik.handleSubmit} className="registration-form">
      <label>Name</label>
      <input type="text" {...formik.getFieldProps('name')} />
      {formik.touched.name && formik.errors.name ? <div className="error">{formik.errors.name}</div> : null}
      
      <label>Email</label>
      <input type="email" {...formik.getFieldProps('email')} />
      {formik.touched.email && formik.errors.email ? <div className="error">{formik.errors.email}</div> : null}
      
      <label>Password</label>
      <input type="password" {...formik.getFieldProps('password')} />
      {formik.touched.password && formik.errors.password ? <div className="error">{formik.errors.password}</div> : null}
      
      <label>Confirm Password</label>
      <input type="password" {...formik.getFieldProps('confirmPassword')} />
      {formik.touched.confirmPassword && formik.errors.confirmPassword ? <div className="error">{formik.errors.confirmPassword}</div> : null}
      
      <button type="submit">Register</button>
    </form>
  );
};

export default RegistrationForm;








.registration-form {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: auto;
}

.registration-form label {
  margin-top: 10px;
}

.registration-form input {
  padding: 5px;
  margin-top: 5px;
}

.registration-form .error {
  color: red;
  font-size: 12px;
}

.registration-form button {
  margin-top: 20px;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}

.registration-form button:hover {
  background-color: #0056b3;
}
