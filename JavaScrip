let currentInput = '';
let hasResult = false;

function updateDisplay(value) {
  const display = document.getElementById('display');
  display.value = value || '';
}

function appendNumber(num) {
  if (hasResult) {
    currentInput = '';
    hasResult = false;
  }
  // Evita múltiplos zeros no início
  if (currentInput === '0' && num === '0') return;
  currentInput += num;
  updateDisplay(currentInput);
}

function appendOperator(op) {
  hasResult = false;

  if (currentInput === '' && op !== '-') return;

  // Substitui operador anterior se o último char já for operador
  const lastChar = currentInput.slice(-1);
  if (['+', '-', '*', '/'].includes(lastChar)) {
    currentInput = currentInput.slice(0, -1);
  }

  // Ponto decimal: evita duplicar
  if (op === '.') {
    // Pega o último número digitado
    const parts = currentInput.split(/[\+\-\*\/]/);
    const lastPart = parts[parts.length - 1];
    if (lastPart.includes('.')) return;
    if (lastPart === '') {
      currentInput += '0';
    }
  }

  currentInput += op;
  updateDisplay(currentInput);
}

function clearDisplay() {
  currentInput = '';
  hasResult = false;
  updateDisplay('');
}

function deleteLast() {
  if (hasResult) {
    clearDisplay();
    return;
  }
  currentInput = currentInput.slice(0, -1);
  updateDisplay(currentInput);
}

function calculate() {
  if (currentInput === '') return;

  try {
    // Remove operador solto no final
    let expression = currentInput.replace(/[\+\-\*\/\.]$/, '');

    // Troca × por * caso necessário (já está como * no HTML, mas por segurança)
    expression = expression.replace(/×/g, '*');

    // Divisão por zero
    if (/\/\s*0(?!\d)/.test(expression)) {
      updateDisplay('Erro: ÷0');
      currentInput = '';
      return;
    }

    const result = Function('"use strict"; return (' + expression + ')')();

    if (!isFinite(result)) {
      updateDisplay('Erro');
      currentInput = '';
      return;
    }

    // Limita casas decimais para evitar números gigantes
    const formatted = parseFloat(result.toFixed(10)).toString();
    currentInput = formatted;
    updateDisplay(formatted);
    hasResult = true;
  } catch (e) {
    updateDisplay('Erro');
    currentInput = '';
  }
}

// Suporte a teclado
document.addEventListener('keydown', (e) => {
  if (e.key >= '0' && e.key <= '9') appendNumber(e.key);
  else if (e.key === '+') appendOperator('+');
  else if (e.key === '-') appendOperator('-');
  else if (e.key === '*') appendOperator('*');
  else if (e.key === '/') { e.preventDefault(); appendOperator('/'); }
  else if (e.key === '.') appendOperator('.');
  else if (e.key === 'Enter') calculate();
  else if (e.key === 'Backspace') deleteLast();
  else if (e.key === 'Escape') clearDisplay();
});
