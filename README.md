 ![Capture d'écran 2025-04-29 124328](https://github.com/user-attachments/assets/42ac9c8a-b4c7-4238-b9e5-4506895c325d)
 ![Capture d'écran 2025-04-29 124431](https://github.com/user-attachments/assets/a27a0165-e1f2-435c-b404-d8e83b48a65d)
https://avyana--da7ece46276411f086ff569c3dd06744.web.val.run/
# Mental-Health-Application
Projects made with AI, it is created to check up on  patients daily and tells them if they need medical support or not!
/** @jsxImportSource https://esm.sh/react@18.2.0 */
import React, { useState } from "https://esm.sh/react@18.2.0";
import { createRoot } from "https://esm.sh/react-dom@18.2.0/client";

const MOOD_QUESTIONS = [
  "How often do you feel sad or hopeless?",
  "Are you experiencing changes in sleep patterns?", 
  "Do you have difficulty concentrating?",
  "Have you lost interest in activities you used to enjoy?",
  "Are you experiencing significant anxiety?"
];

const SEVERITY_LEVELS = [
  "Low Risk",
  "Moderate Concern",
  "High Risk - Seek Help"
];

function MoodAssessmentApp() {
  const [responses, setResponses] = useState(new Array(MOOD_QUESTIONS.length).fill(0));
  const [assessment, setAssessment] = useState(null);

  const handleResponseChange = (index, value) => {
    const newResponses = [...responses];
    newResponses[index] = value;
    setResponses(newResponses);
  };

  const calculateMoodScore = () => {
    return responses.reduce((a, b) => a + b, 0);
  };

  const assessMood = () => {
    const score = calculateMoodScore();
    let severity = 0;

    if (score > 10) severity = 2;
    else if (score > 5) severity = 1;

    setAssessment({
      score,
      level: SEVERITY_LEVELS[severity],
      recommendation: severity === 2 
        ? "🚨 We strongly recommend consulting a mental health professional immediately." 
        : severity === 1
          ? "⚠️ Consider scheduling a consultation with a therapist or counselor."
          : "✅ Your mood appears stable, but continue self-care practices."
    });
  };

  return (
    <div style={{
      maxWidth: '600px', 
      margin: 'auto', 
      padding: '20px', 
      fontFamily: 'Arial, sans-serif'
    }}>
      <h1>🧠 Mental Wellness Check</h1>
      {MOOD_QUESTIONS.map((question, index) => (
        <div key={index} style={{ marginBottom: '15px' }}>
          <p>{question}</p>
          <div>
            {[0, 1, 2, 3].map(level => (
              <label key={level} style={{ marginRight: '10px' }}>
                <input 
                  type="radio" 
                  name={`q${index}`} 
                  value={level}
                  onChange={() => handleResponseChange(index, level)}
                />
                {['Never', 'Rarely', 'Sometimes', 'Often'][level]}
              </label>
            ))}
          </div>
        </div>
      ))}
      <button 
        onClick={assessMood}
        style={{
          backgroundColor: '#4CAF50',
          color: 'white',
          padding: '10px 20px',
          border: 'none',
          borderRadius: '5px',
          cursor: 'pointer'
        }}
      >
        Assess My Mood
      </button>
      
      {assessment && (
        <div style={{ 
          marginTop: '20px', 
          padding: '15px', 
          backgroundColor: '#f0f0f0', 
          borderRadius: '5px' 
        }}>
          <h2>Assessment Result</h2>
          <p>Mood Score: {assessment.score}</p>
          <p>Risk Level: {assessment.level}</p>
          <p>{assessment.recommendation}</p>
        </div>
      )}
      <p style={{ 
        marginTop: '20px', 
        fontSize: '0.8em', 
        color: '#666' 
      }}>
        🚨 If you are experiencing a mental health emergency, please contact emergency services or a crisis helpline.
      </p>
    </div>
  );
}

function client() {
  createRoot(document.getElementById("root")).render(<MoodAssessmentApp />);
}
if (typeof document !== "undefined") { client(); }

export default async function server(request: Request): Promise<Response> {
  return new Response(`
    <html>
      <head>
        <title>Mental Wellness Check</title>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
      </head>
      <body>
        <div id="root"></div>
        <script src="https://esm.town/v/std/catch"></script>
        <script type="module" src="${import.meta.url}"></script>
      </body>
    </html>
  `, {
    headers: { "content-type": "text/html" }
  });
}
