# Bordimport React, { useState } from 'react';
import { View, Text, TextInput, TouchableOpacity, StyleSheet, Vibration } from 'react-native';

const MAX_SCORE = 43;

export default function App() {
  const [spelers, setSpelers] = useState([]);
  const [fase, setFase] = useState('start');
  const [naam, setNaam] = useState('');
  const [huidige, setHuidige] = useState(0);
  const [score, setScore] = useState('');

  const addSpeler = () => {
    if (!naam) return;
    setSpelers([...spelers, { naam, scores: [] }]);
    setNaam('');
  };

  const bevestig = () => {
    const s = Number(score);
    if (s < 0 || s > MAX_SCORE) return;
    const kopie = [...spelers];
    kopie[huidige].scores.push(s);
    setSpelers(kopie);
    setScore('');
    Vibration.vibrate(60);
    setHuidige((huidige + 1) % spelers.length);
  };

  if (fase === 'start') {
    return (
      <View style={styles.container}>
        <Text style={styles.title}>Kastje Gooien</Text>
        <TextInput style={styles.input} placeholder="Spelernaam" value={naam} onChangeText={setNaam} />
        <TouchableOpacity style={styles.button} onPress={addSpeler}>
          <Text style={styles.buttonText}>Toevoegen</Text>
        </TouchableOpacity>
        {spelers.map(s => <Text key={s.naam}>{s.naam}</Text>)}
        <TouchableOpacity style={styles.button} onPress={() => setFase('spel')}>
          <Text style={styles.buttonText}>Start spel</Text>
        </TouchableOpacity>
      </View>
    );
  }

  const speler = spelers[huidige];
  const totaal = speler.scores.reduce((a,b)=>a+b,0);

  return (
    <View style={styles.container}>
      <Text style={styles.big}>{speler.naam}</Text>
      <Text>Totaal: {totaal}</Text>
      <TextInput
        style={styles.input}
        keyboardType="numeric"
        placeholder={`Score 0-${MAX_SCORE}`}
        value={score}
        onChangeText={setScore}
      />
      <TouchableOpacity style={styles.button} onPress={bevestig}>
        <Text style={styles.buttonText}>Bevestig</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex:1, justifyContent:'center', padding:24 },
  title: { fontSize:28, textAlign:'center', marginBottom:16 },
  big: { fontSize:36, fontWeight:'bold', textAlign:'center', marginBottom:16 },
  input: { borderWidth:1, padding:12, marginVertical:8 },
  button: { backgroundColor:'#2563eb', padding:14, marginVertical:8 },
  buttonText: { color:'white', textAlign:'center', fontWeight:'bold' }
});
