// ==UserScript==
// @name         ChatGPT Smart Code Collapse
// @namespace    http://tampermonkey.net/
// @version      1.0.0
// @description  Collapses long code blocks (>20 lines) in ChatGPT.
// @author       github.com/rs4t
// @match        https://chatgpt.com/*
// @match        https://chat.openai.com/*
// @icon         https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/chatgpt.png
// @license      MIT
// @grant        none
// @run-at       document-idle
// ==/UserScript==

(function () {
  "use strict";

  const COLLAPSE_AFTER_LINES = 20;
  const PREVIEW_LINES = 10;

  // Track processed elements
  const processedElements = new WeakSet();

  // Create chevron icon
  const CHEVRON_ICON = `
    <svg width="16" height="16" viewBox="0 0 20 20" fill="currentColor">
      <path fill-rule="evenodd" clip-rule="evenodd"
        d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 111.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z">
      </path>
    </svg>
  `;

  // Add styles
  const style = document.createElement("style");
  style.textContent = `
    .gpt-collapse-wrapper {
      overflow: hidden;
      position: relative;
      transition: max-height 0.25s ease;
    }

    .gpt-collapse-fade {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      height: 80px;
      pointer-events: none;
      background: linear-gradient(to bottom, rgba(0,0,0,0), rgba(32,33,35,1));
      transition: opacity 0.18s ease;
    }

    .gpt-collapse-fade.hidden {
      opacity: 0;
    }

    .gpt-collapse-bar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 8px 16px;
      background: rgba(255,255,255,0.02);
      border-top: 1px solid rgba(255,255,255,0.06);
    }

    .gpt-collapse-btn {
      display: flex;
      align-items: center;
      gap: 8px;
      background: transparent;
      border: none;
      color: rgb(172,172,190);
      cursor: pointer;
      padding: 6px 12px;
      border-radius: 8px;
      font-size: 13px;
      font-family: inherit;
      transition: background 0.15s;
    }

    .gpt-collapse-btn:hover {
      background: rgba(255,255,255,0.06);
    }

    .gpt-collapse-chevron {
      display: inline-flex;
      transition: transform 0.2s;
    }

    .gpt-expanded .gpt-collapse-chevron {
      transform: rotate(90deg);
    }
  `;
  document.head.appendChild(style);

  function getCodeText(preElement) {
    // Try to find the actual code content
    const codeElement = preElement.querySelector('code');
    if (codeElement) return codeElement.innerText;

    // For the new ChatGPT structure, the pre might contain the code directly
    return preElement.innerText;
  }

  function processCodeBlock(pre) {
    // Skip if already processed
    if (processedElements.has(pre)) return;

    // Don't process if it's already inside our wrapper
    if (pre.closest('.gpt-collapse-wrapper')) return;

    // Get the code content
    const codeText = getCodeText(pre);
    if (!codeText || codeText.trim().length < 50) return;

    // Count lines
    const lines = codeText.split('\n');
    if (lines.length <= COLLAPSE_AFTER_LINES) return;

    // Mark as processed
    processedElements.add(pre);

    // Get the parent container
    const parent = pre.parentNode;
    if (!parent) return;

    // Create wrapper
    const wrapper = document.createElement('div');
    wrapper.className = 'gpt-collapse-wrapper';
    wrapper.style.maxHeight = 'auto';

    // Insert wrapper before pre and move pre inside
    parent.insertBefore(wrapper, pre);
    wrapper.appendChild(pre);

    // Calculate collapsed height
    const lineHeight = 24; // Approximate
    const collapsedHeight = lineHeight * PREVIEW_LINES + 50;

    // Set initial collapsed state
    let expanded = false;
    wrapper.style.maxHeight = collapsedHeight + 'px';
    wrapper.style.overflow = 'hidden';

    // Create fade element
    const fade = document.createElement('div');
    fade.className = 'gpt-collapse-fade';
    wrapper.appendChild(fade);

    // Create controls bar
    const controls = document.createElement('div');
    controls.className = 'gpt-collapse-bar';

    // Create toggle button
    const toggleBtn = document.createElement('button');
    toggleBtn.className = 'gpt-collapse-btn';
    toggleBtn.innerHTML = `
      <span class="gpt-collapse-chevron">${CHEVRON_ICON}</span>
      <span>Show code (${lines.length} lines)</span>
    `;

    toggleBtn.addEventListener('click', () => {
      expanded = !expanded;

      if (expanded) {
        wrapper.style.maxHeight = pre.scrollHeight + 100 + 'px';
        controls.classList.add('gpt-expanded');
        fade.classList.add('hidden');
        toggleBtn.querySelector('span:last-child').textContent = 'Hide code';
      } else {
        wrapper.style.maxHeight = collapsedHeight + 'px';
        controls.classList.remove('gpt-expanded');
        fade.classList.remove('hidden');
        toggleBtn.querySelector('span:last-child').textContent = `Show code (${lines.length} lines)`;
      }
    });

    controls.appendChild(toggleBtn);
    wrapper.parentNode.insertBefore(controls, wrapper.nextSibling);
  }

  function scan() {
    // Find all pre elements that contain code
    const pres = document.querySelectorAll('pre');

    pres.forEach(pre => {
      // Check if it contains actual code (not just empty)
      const code = pre.querySelector('code');
      if (!code && !pre.innerText.trim()) return;

      processCodeBlock(pre);
    });
  }

  // Debounced scan
  let scanTimeout;
  function debouncedScan() {
    if (scanTimeout) clearTimeout(scanTimeout);
    scanTimeout = setTimeout(scan, 200);
  }

  // Set up observer
  const observer = new MutationObserver((mutations) => {
    for (const mutation of mutations) {
      if (mutation.addedNodes.length > 0) {
        debouncedScan();
        break;
      }
    }
  });

  observer.observe(document.body, {
    childList: true,
    subtree: true
  });

  // Initial scan
  setTimeout(scan, 1000);
  setTimeout(scan, 3000); // Second pass for dynamic content
})();
