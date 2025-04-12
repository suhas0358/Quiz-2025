// Be an Ambedkaritee – Quiz 2025 React App (Bilingual: English + Marathi) // Main structure: Home, Quiz, Certificate, Gmail Auth

import React, { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { GoogleLogin } from "@react-oauth/google"; import html2canvas from "html2canvas"; import jsPDF from "jspdf";

const quizData = [ { question: "What is the significance of April 14 in India?", marathi: "१४ एप्रिल भारतात कोणत्या निमित्ताने साजरा केला जातो?", options: [ "Republic Day", "Ambedkar Jayanti", "Constitution Day", "Labour Day" ], answer: "Ambedkar Jayanti" }, { question: "Which Article of the Indian Constitution abolishes untouchability?", marathi: "भारतीय संविधानातील कोणता अनुच्छेद अस्पृश्यतेवर बंदी घालतो?", options: ["Article 17", "Article 21", "Article 19", "Article 370"], answer: "Article 17" }, { question: "What is the name of the book written by Dr. B. R. Ambedkar?", marathi: "डॉ. बाबासाहेब आंबेडकर यांनी लिहिलेल्या पुस्तकाचे नाव काय आहे?", options: ["Discovery of India", "My Experiments with Truth", "Annihilation of Caste", "India Unbound"], answer: "Annihilation of Caste" }, { question: "Which position did Dr. Ambedkar hold in the Constitution drafting committee?", marathi: "संविधान मसुदा समितीमध्ये डॉ. आंबेडकर कोणत्या पदावर होते?", options: ["Chairman", "Secretary", "Member", "President"], answer: "Chairman" }, { question: "What does the term 'Ambedkarite' refer to?", marathi: "'आंबेडकरी' या शब्दाचा अर्थ काय होतो?", options: ["A follower of equality principles", "A political party member", "A constitution writer", "A social worker"], answer: "A follower of equality principles" }, { question: "Which movement is Dr. Ambedkar known for leading?", marathi: "डॉ. आंबेडकर कोणत्या चळवळीचे नेतृत्व करत होते?", options: ["Quit India Movement", "Dalit Buddhist Movement", "Swadeshi Movement", "Civil Disobedience"], answer: "Dalit Buddhist Movement" }, { question: "Which community did Dr. Ambedkar work for the upliftment of?", marathi: "डॉ. आंबेडकर कोणत्या समाजाच्या उत्थानासाठी कार्यरत होते?", options: ["Women", "Farmers", "Dalits", "Workers"], answer: "Dalits" }, { question: "In which year did Dr. Ambedkar embrace Buddhism?", marathi: "डॉ. आंबेडकरांनी कोणत्या वर्षी बौद्ध धर्म स्वीकारला?", options: ["1947", "1950", "1956", "1958"], answer: "1956" }, { question: "What is the central theme of Dr. Ambedkar’s philosophy?", marathi: "डॉ. आंबेडकरांच्या तत्त्वज्ञानाचा मुख्य विषय काय आहे?", options: ["Economic Growth", "Education for all", "Social Justice and Equality", "Political Campaigning"], answer: "Social Justice and Equality" }, { question: "Which slogan is associated with Dr. B. R. Ambedkar?", marathi: "डॉ. आंबेडकरांशी संबंधित घोषवाक्य कोणते आहे?", options: ["Swaraj is my birthright", "Jai Jawan Jai Kisan", "Educate, Agitate, Organize", "Vande Mataram"], answer: "Educate, Agitate, Organize" } ];

export default function AmbedkarQuiz() { const [step, setStep] = useState("login"); const [name, setName] = useState(""); const [email, setEmail] = useState(""); const [answers, setAnswers] = useState(Array(quizData.length).fill(null)); const [score, setScore] = useState(0);

const handleLogin = (credentialResponse) => { const user = JSON.parse(atob(credentialResponse.credential.split('.')[1])); setName(user.name); setEmail(user.email); setStep("quiz"); };

const handleAnswer = (index, option) => { const newAnswers = [...answers]; newAnswers[index] = option; setAnswers(newAnswers); };

const handleSubmit = () => { const result = answers.filter((ans, i) => ans === quizData[i].answer); setScore(result.length); setStep("certificate"); };

const generateCertificate = () => { html2canvas(document.querySelector("#certificate"), { scale: 2 }).then((canvas) => { const imgData = canvas.toDataURL("image/png"); const pdf = new jsPDF(); pdf.addImage(imgData, "PNG", 10, 10, 190, 270); pdf.save("Ambedkaritee_Certificate.pdf"); }); };

return ( <div className="p-4 max-w-2xl mx-auto"> <h1 className="text-3xl font-bold text-center mb-4"> Be an Ambedkaritee – Quiz 2025 <br /> <span className="text-sm">अभिवादन अंबेडकरी विचारांना – प्रश्नमंजुषा २०२५</span> </h1>

{step === "login" && (
    <div className="text-center">
      <p className="mb-4">Login with Gmail to attempt the quiz / जीमेल वापरून लॉगिन करा</p>
      <GoogleLogin onSuccess={handleLogin} onError={() => alert("Login Failed")} />
    </div>
  )}

  {step === "quiz" && (
    <div>
      {quizData.map((q, i) => (
        <Card key={i} className="mb-4">
          <CardContent>
            <p className="font-semibold">Q{i + 1}: {q.question}</p>
            <p className="text-sm text-gray-600">{q.marathi}</p>
            <div className="mt-2 space-y-1">
              {q.options.map((opt) => (
                <div key={opt}>
                  <input
                    type="radio"
                    name={`q${i}`}
                    value={opt}
                    onChange={() => handleAnswer(i, opt)}
                    checked={answers[i] === opt}
                  /> {opt}
                </div>
              ))}
            </div>
          </CardContent>
        </Card>
      ))}
      <Button className="w-full mt-4" onClick={handleSubmit}>
        Submit Quiz / प्रश्नमंजुषा सबमिट करा
      </Button>
    </div>
  )}

  {step === "certificate" && (
    <div className="text-center">
      <div id="certificate" className="bg-white p-6 border shadow-md">
        <img src="/ambedkar_unity.png" alt="Ambedkar" className="mx-auto w-32 mb-4" />
        <h2 className="text-xl font-bold">Certificate of Participation</h2>
        <p className="text-sm">प्रश्नमंजुषेत सहभागासाठी प्रमाणपत्र</p>
        <p className="mt-2">This is to certify that</p>
        <h3 className="font-semibold text-lg">{name}</h3>
        <p>has successfully participated in</p>
        <p><strong>Be an Ambedkaritee – Quiz 2025</strong></p>
        <p>Date: 13/04/2025</p>
        <p className="mt-4 font-bold">
          शिव शाहू फुले आंबेडकर विचार मंच पिंपळगाव सोनारा
        </p>
      </div>
      <Button className="mt-4" onClick={generateCertificate}>Download Certificate</Button>
    </div>
  )}
</div>

); }

