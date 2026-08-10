const debugBtn = document.getElementById('debugBtn');
const loader = document.getElementById('loader');
const results = document.getElementById('results');
const errorBox = document.getElementById('errorBox');
const fixElement = document.getElementById('fix');
const languageSelect = document.getElementById('language');
const historyList = document.getElementById('historyList');

const API_URL = 'https://debuglens-api.onrender.com/debug'; // Render URL
const HISTORY_KEY = 'debuglens_history';

window.addEventListener('load', () => {
  loadHistory();
  loadFromURL();
