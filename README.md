# chat-appimport React, { useState } from 'react';
import { View, Text, TextInput, TouchableOpacity, FlatList, Image, StyleSheet } from 'react-native';

// Dummy data for chats
const chats = [
  { id: '1', name: 'Alice', lastMessage: 'Hey!', time: '10:00' },
  { id: '2', name: 'Bob', lastMessage: 'How are you?', time: '09:30' },
];

export default function App() {
  const [screen, setScreen] = useState('Login'); // Login, ChatList, ChatScreen, Settings
  const [phone, setPhone] = useState('');
  const [messages, setMessages] = useState([
    { id: '1', text: 'Hello!', fromUser: true },
    { id: '2', text: 'Hi there!', fromUser: false },
  ]);
  const [inputText, setInputText] = useState('');

  // Login Screen
  if (screen === 'Login') {
    return (
      <View style={styles.container}>
        <Image source={require('./assets/logo.png')} style={styles.logo} />
        <TextInput
          placeholder="Phone or Email"
          placeholderTextColor="#ffffff"
          style={styles.input}
          value={phone}
          onChangeText={setPhone}
        />
        <TouchableOpacity style={styles.loginButton} onPress={() => setScreen('ChatList')}>
          <Text style={styles.loginText}>Login</Text>
        </TouchableOpacity>
        <TouchableOpacity onPress={() => console.log('Switch language pressed')}>
          <Text style={styles.switchText}>Switch Language</Text>
        </TouchableOpacity>
      </View>
    );
  }

  // Chat List Screen
  if (screen === 'ChatList') {
    return (
      <View style={styles.container}>
        <Text style={styles.header}>Chats</Text>
        <FlatList
          data={chats}
          keyExtractor={(item) => item.id}
          renderItem={({ item }) => (
            <TouchableOpacity style={styles.chatItem} onPress={() => setScreen('ChatScreen')}>
              <Text style={styles.chatName}>{item.name}</Text>
              <Text style={styles.chatMessage}>{item.lastMessage}</Text>
              <Text style={styles.chatTime}>{item.time}</Text>
            </TouchableOpacity>
          )}
        />
        <TouchableOpacity style={styles.settingsButton} onPress={() => setScreen('Settings')}>
          <Text style={styles.loginText}>Settings</Text>
        </TouchableOpacity>
      </View>
    );
  }

  // Chat Screen
  if (screen === 'ChatScreen') {
    return (
      <View style={styles.container}>
        <Text style={styles.header}>Chat with Alice</Text>
        <FlatList
          data={messages}
          keyExtractor={(item) => item.id}
          renderItem={({ item }) => (
            <View style={[styles.messageBubble, item.fromUser ? styles.userBubble : styles.contactBubble]}>
              <Text style={styles.messageText}>{item.text}</Text>
            </View>
          )}
        />
        <View style={styles.inputRow}>
          <TextInput
            style={styles.messageInput}
            placeholder="Type a message"
            placeholderTextColor="#ffffff"
            value={inputText}
            onChangeText={setInputText}
          />
          <TouchableOpacity onPress={() => {
            if(inputText.trim() !== ''){
              setMessages([...messages, { id: Date.now().toString(), text: inputText, fromUser: true }]);
              setInputText('');
            }
          }}>
            <Text style={styles.sendButton}>Send</Text>
          </TouchableOpacity>
        </View>
      </View>
    );
  }

  // Settings Screen
  if(screen === 'Settings') {
    return (
      <View style={styles.container}>
        <Text style={styles.header}>Settings</Text>
        <TouchableOpacity onPress={() => setScreen('ChatList')} style={{marginTop:20}}>
          <Text style={styles.switchText}>Back to Chats</Text>
        </TouchableOpacity>
      </View>
    )
  }

  return null;
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#000000', alignItems: 'center', padding: 20 },
  logo: { width: 120, height: 120, marginBottom: 40 },
  input: { borderColor: '#40E0D0', borderWidth: 1, color: '#ffffff', width: '100%', padding: 10, borderRadius: 8, marginBottom: 20 },
  loginButton: { backgroundColor: '#40E0D0', padding: 15, borderRadius: 10, width: '100%', alignItems: 'center', marginBottom: 15 },
  loginText: { color: '#ffffff', fontSize: 16 },
  switchText: { color: '#ffffff', fontSize: 14 },
  header: { color: '#ffffff', fontSize: 24, marginBottom: 20 },
  chatItem: { borderBottomColor: '#40E0D0', borderBottomWidth: 1, paddingVertical: 10, width: '100%' },
  chatName: { color: '#ffffff', fontSize: 18 },
  chatMessage: { color: '#40E0D0' },
  chatTime: { color: '#ffffff', fontSize: 12 },
  settingsButton: { backgroundColor: '#40E0D0', padding: 10, borderRadius: 8, marginTop: 20 },
  messageBubble: { padding: 10, borderRadius: 10, marginVertical: 5, maxWidth: '80%' },
  userBubble: { backgroundColor: '#40E0D0', alignSelf: 'flex-end' },
  contactBubble: { backgroundColor: '#333333', alignSelf: 'flex-start' },
  messageText: { color: '#ffffff' },
  inputRow: { flexDirection: 'row', width: '100%', marginTop: 10, alignItems: 'center' },
  messageInput: { flex: 1, borderColor: '#40E0D0', borderWidth: 1, borderRadius: 8, padding: 10, color: '#ffffff' },
  sendButton: { color: '#ffffff', padding: 10, marginLeft: 5 },
});