---
layout: archive
title: "Life"
permalink: /life/
author_profile: true
---

A glimpse of my life outside research.

More updates and photos coming soon.

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
