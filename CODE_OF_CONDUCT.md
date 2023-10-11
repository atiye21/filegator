<?php
// Replace with your Telegram Bot API token
$token = '6545666142:AAF1E-SnJYLlce-OoFZ0vXlO-yMyJargZPM';

// Function to send a message through the bot
function sendMessage($chat_id, $text) {
    global $token;
    $url = "https://api.telegram.org/bot$token/sendMessage?chat_id=$chat_id&text=" . urlencode($text);
    file_get_contents($url);
}

// Function to process and replace usernames in a message
function processMessage($message) {
    // Define the desired username to replace with
    $desiredUsername = '@sadrsh_bot';

    // Regular expression to match and replace usernames
    $pattern = '/@(\w+)/';
    $replacement = $desiredUsername;
    $processedMessage = preg_replace($pattern, $replacement, $message);

    return $processedMessage;
}

// Retrieve updates from the Telegram API
$update = json_decode(file_get_contents("php://input"), TRUE);

if (isset($update["message"])) {
    $message = $update["message"];
    $chat_id = $message["chat"]["id"];
    $text = $message["text"];

    // Process the message and replace usernames
    $processedMessage = processMessage($text);

    // Send the processed message back to the user
    sendMessage($chat_id, $processedMessage);
}
