<style>
.hover-hash {
  position: relative;
  cursor: pointer;
  scroll-margin-top: 40vh;
}

.hover-hash::before {
  content: "#";
  opacity: 0;
  margin-right: 8px;
  transition: opacity 0.3s ease;
  color: #ff0044; / color del símbolo /
}

.hover-hash:hover::before {
  opacity: 1;
}

html {
  scroll-behavior: smooth;
}
</style>

<h1 id="red-team" class="hover-hash">About me</h1>

<script>
document.querySelectorAll('.hover-hash').forEach(header => {
  header.addEventListener('click', () => {
    header.scrollIntoView({
      behavior: 'smooth',
      block: 'center'
    });
  });
});
</script>

```bash
whoami
scriptos0
```

**Hi!,** I am developing my skills in **penetration testing,** with a focus on discovering and exploiting **security vulnerabilities in web applications, networks, and systems.** My goal is to understand attack vectors and defense mechanisms through hands-on practice and continuous learning. I work with tools such as **Burp Suite, Nmap, Metasploit, and Wireshark,** following **OWASP** and **ethical hacking** methodologies. I’m passionate about offensive security, vulnerability assessment, and improving my technical knowledge to perform realistic and effective security testing.

I created this site to share my **write-ups, notes, and experiences in cybersecurity** — from **CTF** challenges to **real-world pentesting** concepts. This is my space to document what I learn, improve my skills, and hopefully help others along the way.

**From:** ![Argentina](/flags/Argentina.jpg)
