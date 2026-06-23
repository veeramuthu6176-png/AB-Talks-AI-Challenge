<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<title>A. Veeramuthu | AI & Data Science Portfolio</title>
<meta name="description" content="A. Veeramuthu - Artificial Intelligence & Data Science Student, Machine Learning Enthusiast, GenAI Builder, and AI/ML Internship Aspirant." />
<meta name="keywords" content="Veeramuthu, AI Engineer, Machine Learning, Data Science, Portfolio, GenAI, Python, TensorFlow" />
<meta name="author" content="A. Veeramuthu" />

<script src="https://cdn.tailwindcss.com"></script>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">

<style>
*{
font-family:'Inter',sans-serif;
scroll-behavior:smooth;
}

body{
transition:all .3s ease;
}

.glass{
backdrop-filter: blur(15px);
}

.gradient-text{
background:linear-gradient(90deg,#06b6d4,#8b5cf6);
-webkit-background-clip:text;
-webkit-text-fill-color:transparent;
}

.project-card{
transition:.3s ease;
}

.project-card:hover{
transform:translateY(-8px);
}

.skill-bar{
animation:grow 2s ease forwards;
transform-origin:left;
}

@keyframes grow{
from{
transform:scaleX(0);
}
to{
transform:scaleX(1);
}
}

.fade-up{
opacity:0;
transform:translateY(30px);
transition:.7s ease;
}

.fade-up.show{
opacity:1;
transform:translateY(0);
}

.cursor{
animation:blink 1s infinite;
}

@keyframes blink{
50%{
opacity:0;
}
}

.active-link{
color:#06b6d4 !important;
}
</style>
</head>

<body class="bg-slate-950 text-white">

<!-- NAVBAR -->
<nav class="fixed top-0 left-0 right-0 z-50 bg-slate-950/80 backdrop-blur-lg border-b border-slate-800">
<div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">

<h1 class="font-bold text-xl gradient-text">
Veeramuthu
</h1>

<div class="hidden md:flex gap-8 text-sm font-medium">
<a href="#home" class="nav-link">Home</a>
<a href="#about" class="nav-link">About</a>
<a href="#skills" class="nav-link">Skills</a>
<a href="#projects" class="nav-link">Projects</a>
<a href="#achievements" class="nav-link">Achievements</a>
<a href="#contact" class="nav-link">Contact</a>
</div>

<button id="themeBtn"
class="px-4 py-2 rounded-lg bg-cyan-500 text-black font-semibold">
☀
</button>

</div>
</nav>

<!-- HERO -->
<section id="home" class="min-h-screen flex items-center justify-center px-6">

<div class="text-center max-w-4xl">

<div class="inline-block px-4 py-2 rounded-full bg-cyan-500/10 border border-cyan-500/30 mb-6">
🚀 Open to AI/ML Internship Opportunities
</div>

<h1 class="text-5xl md:text-7xl font-black mb-4">
A.
<span class="gradient-text">Veeramuthu</span>
</h1>

<h2 class="text-xl md:text-2xl text-slate-300 mb-6">
<span id="typing"></span>
<span class="cursor">|</span>
</h2>

<p class="text-slate-400 max-w-2xl mx-auto leading-relaxed">
Artificial Intelligence & Data Science student at Anna University.
Passionate about Machine Learning, Deep Learning, Neural Networks,
Large Language Models, and building AI products that solve real-world problems.
</p>

<div class="flex justify-center gap-4 mt-8 flex-wrap">

<a href="mailto:veeramuthu6176@gmail.com"
class="px-6 py-3 rounded-xl bg-cyan-500 text-black font-semibold">
Email Me
</a>

<a href="https://github.com/veeramuthu6176-png"
target="_blank"
class="px-6 py-3 rounded-xl border border-slate-700">
GitHub
</a>

<a href="https://linkedin.com/in/veeramuthu-ai"
target="_blank"
class="px-6 py-3 rounded-xl border border-slate-700">
LinkedIn
</a>

</div>

</div>

</section>

<!-- ABOUT -->
<section id="about" class="py-24 px-6 fade-up">
<div class="max-w-6xl mx-auto">

<h2 class="text-4xl font-bold mb-10 gradient-text">
About Me
</h2>

<div class="grid md:grid-cols-2 gap-10">

<div>
<p class="text-slate-300 leading-8">
I am a B.Tech Artificial Intelligence and Data Science student
at Sri Venkateswara College of Engineering and Technology (Anna University)
with a CGPA of 8.5.
</p>

<p class="text-slate-300 leading-8 mt-5">
I actively build in public through my 60 Days of AI challenge where
I document AI, Machine Learning, Deep Learning, and LLM projects.
My goal is to become a high-impact AI Engineer and contribute
to innovative startup ecosystems.
</p>
</div>

<div class="grid grid-cols-2 gap-5">

<div class="bg-slate-900 p-6 rounded-2xl">
<h3 class="text-3xl font-bold text-cyan-400">8.5</h3>
<p>CGPA</p>
</div>

<div class="bg-slate-900 p-6 rounded-2xl">
<h3 class="text-3xl font-bold text-purple-400">60+</h3>
<p>Days AI Challenge</p>
</div>

<div class="bg-slate-900 p-6 rounded-2xl">
<h3 class="text-3xl font-bold text-cyan-400">2027</h3>
<p>Graduation</p>
</div>

<div class="bg-slate-900 p-6 rounded-2xl">
<h3 class="text-3xl font-bold text-purple-400">AI/ML</h3>
<p>Career Focus</p>
</div>

</div>

</div>

</div>
</section>

<!-- SKILLS -->
<section id="skills" class="py-24 px-6 bg-slate-900/40 fade-up">

<div class="max-w-6xl mx-auto">

<h2 class="text-4xl font-bold mb-12 gradient-text">
Skills
</h2>

<div class="grid md:grid-cols-2 gap-10">

<div>

<div class="mb-6">
<div class="flex justify-between mb-2">
<span>Python</span>
<span>90%</span>
</div>
<div class="bg-slate-800 h-3 rounded-full">
<div class="skill-bar bg-cyan-500 h-3 rounded-full w-[90%]"></div>
</div>
</div>

<div class="mb-6">
<div class="flex justify-between mb-2">
<span>Machine Learning</span>
<span>85%</span>
</div>
<div class="bg-slate-800 h-3 rounded-full">
<div class="skill-bar bg-purple-500 h-3 rounded-full w-[85%]"></div>
</div>
</div>

<div class="mb-6">
<div class="flex justify-between mb-2">
<span>Deep Learning</span>
<span>80%</span>
</div>
<div class="bg-slate-800 h-3 rounded-full">
<div class="skill-bar bg-cyan-500 h-3 rounded-full w-[80%]"></div>
</div>
</div>

<div class="mb-6">
<div class="flex justify-between mb-2">
<span>LLMs & GenAI</span>
<span>78%</span>
</div>
<div class="bg-slate-800 h-3 rounded-full">
<div class="skill-bar bg-purple-500 h-3 rounded-full w-[78%]"></div>
</div>
</div>

</div>

<div>

<div class="flex flex-wrap gap-3">

<span class="px-4 py-2 bg-slate-800 rounded-full">Python</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">NumPy</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Pandas</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Scikit-Learn</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">TensorFlow</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Keras</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">HuggingFace</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Git</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">GitHub</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">VS Code</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Jupyter</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Prompt Engineering</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Leadership</span>
<span class="px-4 py-2 bg-slate-800 rounded-full">Communication</span>

</div>

</div>

</div>

</div>
</section>

<!-- PROJECTS -->
<section id="projects" class="py-24 px-6 fade-up">

<div class="max-w-6xl mx-auto">

<h2 class="text-4xl font-bold mb-12 gradient-text">
Projects
</h2>

<div class="grid md:grid-cols-3 gap-8">

<div class="project-card bg-slate-900 p-6 rounded-3xl">
<h3 class="font-bold text-xl mb-4">
60 Days of AI
</h3>
<p class="text-slate-400 mb-5">
Public AI learning challenge documenting ML, DL,
LLMs and AI engineering concepts.
</p>

<div class="flex flex-wrap gap-2">
<span class="px-3 py-1 bg-cyan-500/20 rounded-full text-sm">Python</span>
<span class="px-3 py-1 bg-cyan-500/20 rounded-full text-sm">ML</span>
<span class="px-3 py-1 bg-cyan-500/20 rounded-full text-sm">GenAI</span>
</div>
</div>

<div class="project-card bg-slate-900 p-6 rounded-3xl">
<h3 class="font-bold text-xl mb-4">
Backpropagation Neural Network
</h3>
<p class="text-slate-400 mb-5">
Built a neural network from scratch to solve XOR
using gradient descent and backpropagation.
</p>

<div class="flex flex-wrap gap-2">
<span class="px-3 py-1 bg-purple-500/20 rounded-full text-sm">NumPy</span>
<span class="px-3 py-1 bg-purple-500/20 rounded-full text-sm">Neural Nets</span>
</div>
</div>

<div class="project-card bg-slate-900 p-6 rounded-3xl">
<h3 class="font-bold text-xl mb-4">
Naan Mudhalvan AI Project
</h3>
<p class="text-slate-400 mb-5">
Academic AI/ML project developed under the
Tamil Nadu Skill Development initiative.
</p>

<div class="flex flex-wrap gap-2">
<span class="px-3 py-1 bg-cyan-500/20 rounded-full text-sm">Python</span>
<span class="px-3 py-1 bg-cyan-500/20 rounded-full text-sm">Machine Learning</span>
</div>
</div>

</div>

</div>
</section>

<!-- ACHIEVEMENTS -->
<section id="achievements" class="py-24 px-6 bg-slate-900/40 fade-up">

<div class="max-w-6xl mx-auto">

<h2 class="text-4xl font-bold mb-12 gradient-text">
Achievements & Certifications
</h2>

<div class="space-y-6">

<div class="bg-slate-900 p-6 rounded-2xl">
🏆 Google Cloud Generative AI Certification
</div>

<div class="bg-slate-900 p-6 rounded-2xl">
🏆 AI Fundamentals — LLMs & Responsible AI
</div>

<div class="bg-slate-900 p-6 rounded-2xl">
🏆 IICT AI Skills House Program
</div>

<div class="bg-slate-900 p-6 rounded-2xl">
🏆 Claude AI Mastery Challenge
</div>

</div>

</div>
</section>

<!-- CONTACT -->
<section id="contact" class="py-24 px-6 fade-up">

<div class="max-w-4xl mx-auto">

<h2 class="text-4xl font-bold mb-10 text-center gradient-text">
Contact Me
</h2>

<form class="space-y-5">

<input
type="text"
placeholder="Your Name"
class="w-full p-4 rounded-xl bg-slate-900 border border-slate-700"
/>

<input
type="email"
placeholder="Your Email"
class="w-full p-4 rounded-xl bg-slate-900 border border-slate-700"
/>

<textarea
rows="5"
placeholder="Your Message"
class="w-full p-4 rounded-xl bg-slate-900 border border-slate-700"></textarea>

<button
type="submit"
class="w-full bg-cyan-500 text-black font-bold py-4 rounded-xl">
Send Message
</button>

</form>

<div class="text-center mt-10 text-slate-400">
<p>📧 veeramuthu6176@gmail.com</p>
<p>📍 Arakkonam, Tamil Nadu, India</p>
</div>

</div>

</section>

<!-- FOOTER -->
<footer class="border-t border-slate-800 py-8 text-center text-slate-500">
© 2026 A. Veeramuthu | AI & Data Science Portfolio
</footer>

<script>

// Typing Animation
const roles = [
"AI & Data Science Student",
"Machine Learning Enthusiast",
"GenAI Builder",
"Future AI Engineer"
];

let roleIndex = 0;
let charIndex = 0;
let deleting = false;

const typing = document.getElementById("typing");

function typeEffect(){

const current = roles[roleIndex];

if(!deleting){
typing.textContent = current.substring(0,charIndex++);
if(charIndex > current.length){
deleting = true;
setTimeout(typeEffect,1200);
return;
}
}else{
typing.textContent = current.substring(0,charIndex--);
if(charIndex < 0){
deleting = false;
roleIndex = (roleIndex + 1) % roles.length;
}
}

setTimeout(typeEffect,deleting ? 40 : 80);
}

typeEffect();

// Scroll Animation
const observer = new IntersectionObserver(entries=>{
entries.forEach(entry=>{
if(entry.isIntersecting){
entry.target.classList.add("show");
}
});
});

document.querySelectorAll(".fade-up").forEach(el=>{
observer.observe(el);
});

// Dark Light Mode
const btn = document.getElementById("themeBtn");

btn.addEventListener("click",()=>{

document.body.classList.toggle("bg-white");
document.body.classList.toggle("text-black");
document.body.classList.toggle("bg-slate-950");
document.body.classList.toggle("text-white");

btn.textContent =
document.body.classList.contains("bg-white")
? "🌙"
: "☀";

});

// Active Nav
const sections = document.querySelectorAll("section");
const navLinks = document.querySelectorAll(".nav-link");

window.addEventListener("scroll",()=>{

let current = "";

sections.forEach(section=>{
const top = section.offsetTop - 120;

if(scrollY >= top){
current = section.getAttribute("id");
}
});

navLinks.forEach(link=>{
link.classList.remove("active-link");

if(link.getAttribute("href")==="#" + current){
link.classList.add("active-link");
}
});

});

</script>

</body>
</html>
