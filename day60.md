const debugBtn = document.getElementById('debugBtn');
const codeInput = document.getElementById('codeInput');
const resultDiv = document.getElementById('result');
const errorDiv = document.getElementById('error');
const loadingDiv = document.getElementById('loading');

const API_URL = 'https://debuglens-api.onrender.com/debug'; // unoda backend url

debugBtn.addEventListener('click', async () => {
  const code = codeInput.value.trim();
  if (!code) {
    showError('Please paste some code + error first');
    return;
  }

  // Show loading
  loadingDiv.classList.remove('hidden');
  resultDiv.classList.add('hidden');
  errorDiv.classList.add('hidden');
  debugBtn.disabled = true;
  debugBtn.innerText = 'Debugging...';

  try {
    const res = await fetch(API_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ code })
    });

    if (!res.ok) throw new Error('Server error. Please try again.');

    const data = await res.json();
    showResult(data.fix);
  } catch (err) {
    showError(err.message);
  } finally {
    loadingDiv.classList.add('hidden');
    debugBtn.disabled = false;
    debugBtn.innerText = 'Debug with AI';
  }
});

function showResult(text) {
  resultDiv.innerText = text;
  resultDiv.classList.remove('hidden');
}

function showError(text) {
  errorDiv.innerText = text;
  errorDiv.classList.remove('hidden');
}
