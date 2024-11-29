<template>
    <div class="terminal">
        <div id="output">
        <div v-for="(line, index) in output" :key="index" class="output-line">
            <span>{{ line }}</span>
        </div>
    </div>
    <div class="input-line">
        <span class="prompt">></span>
        <input
            v-model="input"
            @keydown.enter="handleCommand"
            class="command-line"
            autofocus
            />
        </div>
    </div>
</template>

<style scoped>
.terminal {
    background-color: #000;
    color: #0f0;
    padding: 20px;
    height: 60vh; /* Reduced height (previously 80vh) */
    width: 48vw; /* Reduced width by 20% (previously 60vw) */
    max-width: 1200px; /* Limit the maximum width to 1200px */
    overflow-y: auto;
    overflow-x: hidden;
    border-radius: 8px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.8);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
    margin-right: 1vw;
    font-family: monospace;
}

/* Responsive styles for smaller screens */
@media (max-width: 767px) {
    .terminal {
        height: 70vh; /* Adjusted height for small screens */
        width: 80vw; /* Reduced width further on small screens */
    }
}

/* Style for output lines (commands and responses) */
.output-line {
  margin-bottom: 5px; /* Space between lines */
  word-wrap: break-word; /* Wrap long lines to fit container */
}

/* Input line container */
.input-line {
  display: flex; /* Arrange elements horizontally */
  align-items: center; /* Align elements vertically */
}

/* Terminal prompt text */
.prompt {
  margin-right: 5px; /* Spacing after prompt */
  color: #0f0; /* Same green color as the text */
}

/* Command line input field */
.command-line {
  background: transparent; /* Transparent input background */
  border: none; /* Remove default border */
  color: #0f0; /* Green text for input */
  outline: none; /* Remove focus outline */
  flex: 1; /* Expand to fill available space */
  font-size: 1rem; /* Adjust input text size */
  caret-color: #0f0; /* Green caret for better visibility */
}

/* Scrollbar customization for terminal */
.terminal::-webkit-scrollbar {
  width: 8px; /* Scrollbar width */
}

.terminal::-webkit-scrollbar-track {
  background: #1e1e1e; /* Scrollbar track color */
}

.terminal::-webkit-scrollbar-thumb {
  background: #0f0; /* Scrollbar thumb color */
  border-radius: 4px; /* Rounded scrollbar thumb */
}

.terminal::-webkit-scrollbar-thumb:hover {
  background: #0a0; /* Darker green on hover */
}

</style>





<script>
export default {
    data() {
    return {
      input: "",
      output: [
        "Welcome to Adi's portfolio terminal!",
        "Type 'help' for a list of commands.",
      ],
      commands: {
        help: "Available commands: help, about, skills, projects, resume, contact, clear.",
        about: "Hi! I'm Adi. I am a learning Data Scientist.. <br>Wanna know more? <br>Type 'about --more'",
        skills: "Python, Vue.js, Flask, Data Science, GoLang, PostgreSQL.",
        projects: "Check out my projects: Project A, Project B (add links later).",
        resume: "Opening resume... (add resume link)",
        "about --more" : this.redirectToAbout,
        contact: this.redirectToContact, // Attach the method for contact command
        clear: () => this.clearOutput(),
      },
    };
    },
    methods: {
        handleCommand() {
            const command = this.input.trim();
            const response = this.commands[command] || "Command not recognized. Type 'help' for options.";
            if (typeof response === "function") {
                response();
            } else {
                // Push command first
                this.output.push(`> ${command}`);
                
                // If response is a string, split and push each line
                if (typeof response === 'string') {
                const lines = this.formatOutput(response);
                this.output.push(...lines);
                }
            }
            this.input = ""; // Clear input
        },
        clearOutput() {
            this.output = [];
        },
        redirectToContact() {
            this.output.push("> contact", "Opening contact details...");
            setTimeout(() => {
                this.$router.push('/contact-me');
            }, 1000);
        },
        redirectToAbout() {
            this.output.push("> about --more", "Here's more about me...");
            setTimeout(() => {
                this.$router.push('/about-me');
            }, 1000);
        },
        // Function to replace <br> with actual line breaks
        formatOutput(text) {
            return text.split('<br>');
        },
    },
};
</script>
