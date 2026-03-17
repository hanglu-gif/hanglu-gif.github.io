---
layout: archive
title: "Life"
permalink: /life/
author_profile: true
---

Outside of research, I enjoy spending time on things that help me slow down and stay present.

- 🎹 **Piano** — a relaxing way to focus and reflect.
- 🌊 **Diving** — being in a completely different environment with calmness and control.
- 📷 **Photography** — observing light, patterns, and subtle details in complex scenes.
- ⛳ **Golf** — practicing consistency, patience, and mental focus.
- 🏃‍♀️ **Running** — building endurance while maintaining rhythm and clarity.
- 🚴‍♀️ **Cycling** — enjoying long-distance exploration and steady pacing.
- 🍳 **Cooking** — enjoying the process of creating something simple and comforting.
- ☕ **Coffee (latte art)** — small routines that require care and attention.

✍️ I also enjoy writing from time to time, especially when trying to explain technical ideas in a more intuitive way. Some of my articles have received 1000+ saves, with total views exceeding 10,000. It is always rewarding to see that something I wrote resonated with others.

🌍 I also enjoy traveling, and so far I have had the chance to visit more than 30 countries. Experiencing different cultures and perspectives has gradually shaped how I think, work, and relate to people.

These experiences continue to remind me to be with curiosity, patience, and a broader perspective.

## Visitor Guestbook

<div id="guestbook" class="guestbook">
  <form id="guestbook-form" class="guestbook__form">
    <label for="guestbook-name">Name</label>
    <input id="guestbook-name" name="name" type="text" maxlength="30" placeholder="Your name" required>

    <label for="guestbook-message">Message</label>
    <textarea id="guestbook-message" name="message" rows="4" maxlength="280" placeholder="Leave a message..." required></textarea>

    <button type="submit" class="btn btn--primary">Submit</button>
    <p id="guestbook-tip" class="guestbook__tip">Messages are saved in this browser (local guestbook).</p>
  </form>

  <div>
    <h3>Recent messages</h3>
    <ul id="guestbook-list" class="guestbook__list"></ul>
  </div>
</div>

<style>
  .guestbook {
    display: grid;
    gap: 1.25rem;
    margin-top: 1rem;
  }

  .guestbook__form {
    display: grid;
    gap: 0.65rem;
    max-width: 42rem;
  }

  .guestbook__tip {
    margin: 0;
    font-size: 0.875rem;
    opacity: 0.8;
  }

  .guestbook__list {
    list-style: none;
    margin: 0;
    padding: 0;
    display: grid;
    gap: 0.75rem;
  }

  .guestbook__item {
    border: 1px solid #d8d8d8;
    border-radius: 0.5rem;
    padding: 0.75rem 0.9rem;
    background: #fff;
  }

  .guestbook__meta {
    display: flex;
    justify-content: space-between;
    gap: 1rem;
    font-size: 0.9rem;
    margin-bottom: 0.35rem;
    opacity: 0.8;
  }

  .guestbook__message {
    margin: 0;
    white-space: pre-wrap;
    word-break: break-word;
  }
</style>

<script>
  (function () {
    var storageKey = 'life_guestbook_messages';
    var maxMessages = 30;

    var form = document.getElementById('guestbook-form');
    var list = document.getElementById('guestbook-list');
    if (!form || !list) return;

    function escapeHtml(text) {
      return text
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#039;');
    }

    function readMessages() {
      try {
        var raw = localStorage.getItem(storageKey);
        var parsed = raw ? JSON.parse(raw) : [];
        return Array.isArray(parsed) ? parsed : [];
      } catch (error) {
        return [];
      }
    }

    function saveMessages(messages) {
      localStorage.setItem(storageKey, JSON.stringify(messages.slice(0, maxMessages)));
    }

    function render() {
      var messages = readMessages();
      if (messages.length === 0) {
        list.innerHTML = '<li class="guestbook__item">No messages yet. Be the first one 👋</li>';
        return;
      }

      list.innerHTML = messages
        .map(function (item) {
          return (
            '<li class="guestbook__item">' +
            '<div class="guestbook__meta"><strong>' + escapeHtml(item.name) + '</strong><span>' + escapeHtml(item.time) + '</span></div>' +
            '<p class="guestbook__message">' + escapeHtml(item.message) + '</p>' +
            '</li>'
          );
        })
        .join('');
    }

    form.addEventListener('submit', function (event) {
      event.preventDefault();

      var nameInput = form.elements.name;
      var messageInput = form.elements.message;
      var name = (nameInput.value || '').trim();
      var message = (messageInput.value || '').trim();
      if (!name || !message) return;

      var now = new Date();
      var record = {
        name: name.slice(0, 30),
        message: message.slice(0, 280),
        time: now.toLocaleString()
      };

      var messages = readMessages();
      messages.unshift(record);
      saveMessages(messages);
      form.reset();
      render();
    });

    render();
  })();
</script>
