# Aichatbot 
require('dotenv').config();
const express = require('express');
const bodyParser = require('body-parser');
const { Configuration, OpenAIApi } = require('openai');

const app = express();
app.use(bodyParser.json());

// OpenAI setup
const configuration = new Configuration({
    apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

// AI chat endpoint
app.post('/chat', async (req, res) => {
    const { message } = req.body;

    try {
        const aiResponse = await openai.createCompletion({
            model: 'text-davinci-003',
            prompt: message,
            max_tokens: 150,
        });

        res.json({ reply: aiResponse.data.choices[0].text.trim() });
    } catch (error) {
        console.error('Error:', error);
        res.status(500).send('Error communicating with AI');
    }
});

app.listen(5000, () => {
    console.log('Server running on http://localhost:5000');
});
