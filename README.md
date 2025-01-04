# Voice

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jarvis Voice Assistant</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
      background-color: #222;
      color: white;
    }
    button {
      background-color: #0b6efb;
      color: white;
      border: none;
      padding: 15px 30px;
      cursor: pointer;
      font-size: 16px;
      border-radius: 5px;
      margin-top: 10px;
    }
    button:hover {
      background-color: #094cb7;
    }
  </style>
</head>
<body>
  <h1>Jarvis Voice Assistant</h1>
  <p>Click the button and ask me anything!</p>
  <button id="start-btn">Start Jarvis</button>
  <button id="restart-btn" style="display:none;">Restart Jarvis</button>
  <p id="output"></p>

  <script>
    const startButton = document.getElementById("start-btn");
    const restartButton = document.getElementById("restart-btn");
    const output = document.getElementById("output");

    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();
    const synth = window.speechSynthesis;

    recognition.lang = 'en-US';
    recognition.interimResults = false;
    recognition.continuous = true;

    const responseDictionary = {
      "hello": "Hello! How can I assist you today?",
      "your name": "I am Jarvis, your voice assistant.",
      "time": `The current time is ${new Date().toLocaleTimeString()}.`,
      "date": `Today's date is ${new Date().toLocaleDateString()}.`,
      "weather": "I can’t provide live weather updates right now, but you can check online!",
      "joke": "Why don’t scientists trust atoms? Because they make up everything!",
      "what is my father name": "Your Father Name Is Megh Singh Rathore.",
      "what is my name": "OOH! Your Name Is Kaushal Pratap Sing Rathore.",
      "what is my email": "Your E-mail is kp9985079@gmail.com.",
      "elon musk": "Elon Musk is a billionaire entrepreneur, CEO of SpaceX, Tesla, and Twitter.",
      "what is ai": "Artificial Intelligence is the simulation of human intelligence in machines.",
      "who is the president of usa": "The President of the United States is Joe Biden, as of January 2025.",
      "what is python": "Python is a popular programming language known for its simplicity and versatility.",
      "tell me a joke": "What do you call fake spaghetti? An impasta!",
      "your age": "I am ageless! I exist only to assist you.",
      "how are you": "I am doing well, thank you for asking!",
      "good morning": "Good morning! How can I assist you today?",
      "good evening": "Good evening! What can I help you with?",
      "how to code": "To start coding, choose a language like Python, and begin writing simple programs!",
      "who invented the lightbulb": "Thomas Edison is widely credited with inventing the lightbulb.",
      "who invented the telephone": "Alexander Graham Bell invented the telephone.",
      "who is mark zuckerberg": "Mark Zuckerberg is the co-founder and CEO of Facebook.",
      "how does the internet work": "The internet connects computers and servers worldwide through networks using protocols like HTTP.",
      "define quantum computing": "Quantum computing uses quantum mechanics to process information in ways classical computers cannot.",
      "what is a computer virus": "A computer virus is a malicious software program designed to damage or disrupt systems.",
      "can you tell a story": "Once upon a time, in a faraway land, a curious traveler discovered a secret portal...",
      "who is isaac newton": "Isaac Newton was an English mathematician and physicist, known for formulating the laws of motion.",
      "what is gravity": "Gravity is a force that pulls objects toward the center of the Earth or any other body with mass.",
      "can you sing": "I can't sing, but I can tell you a song's lyrics! Want to hear one?",
      "tell me a riddle": "What has keys but can't open locks? A piano!",
      "who is albert einstein": "Albert Einstein was a theoretical physicist, known for his theory of relativity.",
      "what is relativity": "Relativity is the theory developed by Einstein that describes the relationship between space and time.",
      "who is william shakespeare": "William Shakespeare was an English playwright and poet, widely regarded as one of the greatest writers.",
      "who is oprah winfrey": "Oprah Winfrey is a talk show host, media proprietor, and philanthropist.",
      "what is bitcoin": "Bitcoin is a decentralized digital currency that allows peer-to-peer transactions over the internet.",
      "what is cryptocurrency": "Cryptocurrency is a digital currency that uses cryptography for security and operates independently of a central bank.",
      "what is blockchain": "Blockchain is a decentralized and distributed digital ledger that records transactions across multiple computers.",
      "who is steve jobs": "Steve Jobs was the co-founder of Apple Inc. and a pioneer of personal computing.",
      "what is machine learning": "Machine learning is a type of artificial intelligence that enables systems to learn from data and improve over time.",
      "what is deep learning": "Deep learning is a subset of machine learning that uses neural networks with many layers to analyze data.",
      "write a poem": "Beneath the vast and wide sky, dreams and stars find a place to hide in the quiet shadows. A gentle breeze carries a whispered song, guiding weary hearts along their paths. The moonlight bathes the earth in a soft silver glow, weaving secrets into its shimmering flow. Each quiet night tells a story, rich with love, hope, and the golden hues of autumn leaves. Though the world sleeps, hearts continue to yearn for the dawn, for light, and for dreams that burn brightly with endless possibilities.",
      "what is natural language processing": "Natural Language Processing (NLP) is a field of AI that focuses on the interaction between computers and human language.",
      "what is the cloud": "The cloud is a network of remote servers that store data and applications, allowing access from anywhere.",
      "what is html": "HTML stands for HyperText Markup Language, used to create the structure of web pages.",
      "what is css": "CSS stands for Cascading Style Sheets, used to style and layout web pages.",
      "what is javascript": "JavaScript is a programming language used to create interactive effects within web browsers.",
      "who is jeff bezos": "Jeff Bezos is the founder of Amazon and one of the world's wealthiest individuals.",
      "what is a website": "A website is a collection of web pages that are accessed over the internet through a domain name.",
      "who is elon musk married to": "Elon Musk has been married three times, and his current partner is Grimes, a musician.",
      "how many continents are there": "There are seven continents on Earth: Asia, Africa, North America, South America, Antarctica, Europe, and Australia.",
      "how many countries are there in the world": "There are 195 countries in the world today.",
      "who is the richest person in the world": "As of January 2025, the richest person in the world is Elon Musk.",
      "how tall is mount everest": "Mount Everest is the highest mountain on Earth, standing at 8,848 meters (29,029 feet).",
      "how deep is the mariana trench": "The Mariana Trench is the deepest oceanic point on Earth, reaching a depth of about 36,000 feet (10,973 meters).",
      "what is a black hole": "A black hole is a region of space where the gravitational pull is so strong that not even light can escape it.",
      "how do airplanes fly": "Airplanes fly by using wings to generate lift, and engines to provide thrust, overcoming gravity.",
      "what is a solar eclipse": "A solar eclipse occurs when the moon passes between the Earth and the sun, casting a shadow on the Earth.",
      "what is a lunar eclipse": "A lunar eclipse occurs when the Earth passes between the sun and the moon, causing the moon to appear dark.",
      "how do magnets work": "Magnets work by generating a magnetic field that attracts or repels other magnetic materials.",
      "what is sound": "Sound is a vibration that travels through a medium, such as air, and can be heard by the human ear.",
      "what is light": "Light is electromagnetic radiation that is visible to the human eye and allows us to see objects.",
      "what is electricity": "Electricity is the flow of electric charge, typically through wires, to power devices and systems.",
      "how do computers work": "Computers process data using hardware and software to perform tasks like calculations, storage, and communication.",
      "what is artificial intelligence": "Artificial Intelligence is the field of computer science focused on creating machines that can perform tasks that would normally require human intelligence.",
      "how do plants grow": "Plants grow by absorbing sunlight, water, and carbon dioxide to produce food through photosynthesis.",
      "what is photosynthesis": "Photosynthesis is the process by which plants use sunlight to make their own food from carbon dioxide and water.",
      "how does a plant make food": "Plants make food through photosynthesis, which occurs in the chloroplasts of plant cells.",
      "how do fish breathe": "Fish breathe by taking in water through their mouths, passing it over their gills, and extracting oxygen from it.",
      "what is the human brain": "The human brain is the central organ of the nervous system, responsible for controlling thoughts, memory, emotions, and motor functions.",
      "how do humans breathe": "Humans breathe by taking in oxygen through their nose or mouth, which is absorbed by the lungs and transported through the bloodstream.",
      "who is nicolas tesla": "Nikola Tesla was an inventor and electrical engineer, known for his contributions to the development of alternating current (AC) electricity.",
      "what is an electric car": "An electric car is a vehicle powered by an electric motor, using energy stored in rechargeable batteries.",
      "how do satellites work": "Satellites work by orbiting Earth and transmitting data back to Earth, often used for communication and navigation.",
      "how does a GPS work": "A GPS system uses signals from satellites to determine the location of a device on Earth.",
      "what is the internet of things": "The Internet of Things (IoT) is the interconnection of everyday devices to the internet, allowing them to collect and share data."
    };

    startButton.addEventListener("click", () => {
      output.textContent = "Jarvis is listening...";
      startListening();
      restartButton.style.display = "inline";
    });

    restartButton.addEventListener("click", () => {
      output.textContent = "Restarting Jarvis...";
      restartAssistant();
    });

    function startListening() {
      recognition.start();
    }

    recognition.addEventListener("result", (event) => {
      const userQuery = event.results[0][0].transcript.toLowerCase();
      output.textContent = `You said: "${userQuery}"`;
      jarvisResponse(userQuery);
    });

    function jarvisResponse(query) {
      let response = responseDictionary["default"];
      for (const key in responseDictionary) {
        if (query.includes(key)) {
          response = responseDictionary[key];
          break;
        }
      }
      speak(response);
    }

    function speak(message) {
      const utterance = new SpeechSynthesisUtterance(message);
      synth.speak(utterance);
      utterance.onend = function() {
        recognition.start();
      };
      output.textContent = message;
    }

    recognition.addEventListener("error", (event) => {
      output.textContent = `Error: ${event.error}`;
    });

    recognition.addEventListener("end", () => {
      console.log("Recognition ended");
      recognition.start();
    });

    function restartAssistant() {
      recognition.stop();
      synth.cancel();
      output.textContent = "Jarvis has been restarted!";
      startListening();
    }
  </script>
</body>
</html>
