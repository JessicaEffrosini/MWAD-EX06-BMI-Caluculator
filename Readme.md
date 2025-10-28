## Ex06 BMI Calculator
## Date:28.10.25

## AIM
To create a BMI calculator using React Router 

## ALGORITHM
### STEP 1 State Initialization
Manage the current page (Home or Calculator) using React Router.

### STEP 2 User Input
Accept weight and height inputs from the user.

### STEP 3 BMI Calculation
Calculate the BMI based on user input.

### STEP 4 Categorization
Classify the BMI result into categories (Underweight, Normal weight, Overweight, Obesity).

### STEP 5 Navigation
Navigate between pages using React Router.

## PROGRAM
```
BMIapp.jsx

import React, { useState } from 'react';
import './BMI.css';

export default function BMIApp() {
  const [weight, setWeight] = useState('');
  const [height, setHeight] = useState('');
  const [unit, setUnit] = useState('metric');
  const [result, setResult] = useState(null);

  const calculateBMI = (e) => {
    e.preventDefault();
    const w = parseFloat(weight);
    const h = parseFloat(height);
    if (!w || !h) return setResult({ error: 'Please enter valid numbers.' });

    let bmi;
    if (unit === 'metric') {
      const heightMeters = h / 100;
      bmi = w / (heightMeters * heightMeters);
    } else {
      bmi = (703 * w) / (h * h);
    }

    const rounded = Math.round(bmi * 10) / 10;
    const category = getCategory(rounded);
    setResult({ bmi: rounded, category });
  };

  const getCategory = (bmi) => {
    if (bmi < 18.5) return { label: 'Underweight', color: '--under' };
    if (bmi < 25) return { label: 'Normal', color: '--normal' };
    if (bmi < 30) return { label: 'Overweight', color: '--over' };
    return { label: 'Obese', color: '--obese' };
  };

  const reset = () => {
    setWeight('');
    setHeight('');
    setResult(null);
  };

  return (
    <div className="bmi-root">
      <div className="bmi-card">
        <header className="bmi-header">
          <h1>BMI Calculator</h1>
          <p className="subtitle">Quick, accurate, and beautifully styled</p>
        </header>

        <form className="bmi-form" onSubmit={calculateBMI}>
          <div className="row">
            <label className="radio">
              <input
                type="radio"
                name="unit"
                checked={unit === 'metric'}
                onChange={() => setUnit('metric')}
              />
              Metric (kg, cm)
            </label>
            <label className="radio">
              <input
                type="radio"
                name="unit"
                checked={unit === 'imperial'}
                onChange={() => setUnit('imperial')}
              />
              Imperial (lb, in)
            </label>
          </div>

          <div className="inputs">
            <div className="input-group">
              <label>Weight ({unit === 'metric' ? 'kg' : 'lb'})</label>
              <input
                type="number"
                value={weight}
                onChange={(e) => setWeight(e.target.value)}
                placeholder={unit === 'metric' ? 'e.g. 68' : 'e.g. 150'}
                min="0"
              />
            </div>

            <div className="input-group">
              <label>Height ({unit === 'metric' ? 'cm' : 'in'})</label>
              <input
                type="number"
                value={height}
                onChange={(e) => setHeight(e.target.value)}
                placeholder={unit === 'metric' ? 'e.g. 172' : 'e.g. 68'}
                min="0"
              />
            </div>
          </div>

          <div className="actions">
            <button type="submit" className="btn primary">Calculate</button>
            <button type="button" className="btn" onClick={reset}>Reset</button>
          </div>
        </form>

        <div className="result-area">
          {result && result.error && <div className="result error">{result.error}</div>}
          {result && result.bmi && (
            <div className="result">
              <div className="bmi-number" style={{ borderColor: `var(${result.category.color})` }}>
                {result.bmi}
              </div>
              <div className="bmi-info">
                <strong>{result.category.label}</strong>
                <p>{explain(result.category.label)}</p>
              </div>
            </div>
          )}
          {!result && <div className="hint">Enter your weight and height, choose units, then press Calculate.</div>}
        </div>

        <footer className="credit">Designed with ❤️ • React</footer>
      </div>
    </div>
  );
}

function explain(label) {
  switch (label) {
    case 'Underweight':
      return 'You are below the recommended weight range for your height.';
    case 'Normal':
      return 'Your weight is within the healthy range.';
    case 'Overweight':
      return 'You are above the recommended weight range. Consider lifestyle changes.';
    case 'Obese':
      return 'Health risk may be increased — consult a professional.';
    default:
      return '';
  }
}

```

```
BMI.css

:root {
  --bg: #0f1724;
  --card: linear-gradient(135deg, rgba(255,255,255,0.05), rgba(255,255,255,0.02));
  --glass: rgba(255,255,255,0.1);
  --text: #e6eef8;
  --muted: #9fb2ca;

  --under: #60a5fa;
  --normal: #10b981;
  --over: #f59e0b;
  --obese: #ef4444;
}

body {
  margin: 0;
  font-family: 'Poppins', sans-serif;
  background: var(--bg);
  color: var(--text);
}

.bmi-root {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
}

.bmi-card {
  background: var(--card);
  padding: 2rem 3rem;
  border-radius: 20px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  width: 400px;
  backdrop-filter: blur(10px);
  text-align: center;
}

.bmi-header h1 {
  margin-bottom: 0.3rem;
  background: linear-gradient(90deg, #06b6d4, #3b82f6);
  -webkit-background-clip: text;
  color: transparent;
}

.subtitle {
  color: var(--muted);
  font-size: 0.9rem;
}

.row {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin: 1.2rem 0;
}

.radio {
  cursor: pointer;
  font-size: 0.9rem;
}

.inputs {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.input-group {
  text-align: left;
}

.input-group label {
  font-size: 0.9rem;
  color: var(--muted);
}

.input-group input {
  width: 100%;
  padding: 10px;
  margin-top: 4px;
  border: none;
  border-radius: 8px;
  outline: none;
  background: rgba(255,255,255,0.1);
  color: white;
}

.actions {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-top: 1.2rem;
}

.btn {
  background: rgba(255,255,255,0.1);
  border: none;
  color: white;
  padding: 10px 18px;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s ease;
}

.btn.primary {
  background: linear-gradient(90deg, #3b82f6, #06b6d4);
}

.btn:hover {
  opacity: 0.9;
  transform: scale(1.03);
}

.result-area {
  margin-top: 1.5rem;
}

.bmi-number {
  font-size: 2rem;
  font-weight: bold;
  margin: 10px auto;
  width: fit-content;
  padding: 10px 25px;
  border: 3px solid;
  border-radius: 12px;
}

.bmi-info strong {
  font-size: 1.1rem;
}

.hint {
  color: var(--muted);
  font-size: 0.9rem;
}

.credit {
  font-size: 0.8rem;
  margin-top: 1.5rem;
  color: var(--muted);
}


```

## OUTPUT
<img width="1919" height="1099" alt="Screenshot 2025-10-28 135513" src="https://github.com/user-attachments/assets/cca4bccf-5647-4729-8996-22de97120b25" />

<img width="1871" height="1074" alt="Screenshot 2025-10-28 135530" src="https://github.com/user-attachments/assets/337ea882-283a-41bf-93f6-23908b547080" />
<img width="1805" height="1089" alt="Screenshot 2025-10-28 135552" src="https://github.com/user-attachments/assets/be4ac4a7-b131-4a1b-9c34-be0b6992d74e" />


## RESULT
The program for creating BMI Calculator using React Router is executed successfully.
