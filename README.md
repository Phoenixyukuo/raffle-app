import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { motion } from "framer-motion";

export default function Raffle() {
  const [participants, setParticipants] = useState([]);
  const [name, setName] = useState("");
  const [winner, setWinner] = useState(null);
  const [bonusWinner, setBonusWinner] = useState(null);

  const addParticipant = () => {
    if (name.trim() && !participants.includes(name) && participants.length < 20) {
      setParticipants([...participants, name.trim()]);
      setName("");
    }
  };

  const drawWinner = () => {
    if (participants.length > 0) {
      const randomIndex = Math.floor(Math.random() * participants.length);
      setWinner(participants[randomIndex]);
    }
  };

  const drawBonusWinner = () => {
    if (participants.length > 0) {
      const randomIndex = Math.floor(Math.random() * participants.length);
      setBonusWinner(participants[randomIndex]);
    }
  };

  return (
    <div className="flex flex-col items-center p-6 space-y-6 bg-gradient-to-b from-blue-100 to-blue-300 min-h-screen w-full max-w-sm mx-auto text-center">
      <img src="https://drive.google.com/uc?export=view&id=1N1qO-OmyFcratmK5okD4OQeeFxVoLj3o" alt="TMU OGE Logo" className="w-24 mb-4" />
      <h1 className="text-2xl font-bold text-blue-900">TMU OGE 公開抽獎</h1>
      <div className="flex flex-col w-full space-y-2">
        <Input
          value={name}
          onChange={(e) => setName(e.target.value)}
          placeholder="請輸入您的姓名"
          className="border border-blue-500 p-2 rounded-lg w-full text-center"
        />
        <Button onClick={addParticipant} disabled={participants.length >= 20} className="bg-blue-600 hover:bg-blue-700 text-white p-2 rounded-lg w-full">
          加入抽獎
        </Button>
      </div>
      <Card className="w-full shadow-lg border border-gray-300 bg-white">
        <CardContent className="p-4">
          <h2 className="text-lg font-semibold text-gray-700">參加人員 ({participants.length}/20)</h2>
          <ul className="list-disc pl-5 text-gray-600 text-sm">
            {participants.map((p, index) => (
              <li key={index}>{p}</li>
            ))}
          </ul>
        </CardContent>
      </Card>
      <div className="flex flex-col space-y-3 w-full">
        <Button onClick={drawWinner} disabled={participants.length === 0} className="bg-green-600 hover:bg-green-700 text-white p-2 rounded-lg w-full">
          抽取主獎
        </Button>
        <Button onClick={drawBonusWinner} disabled={participants.length === 0} className="bg-yellow-500 hover:bg-yellow-600 text-white p-2 rounded-lg w-full">
          抽取加碼獎
        </Button>
      </div>
      {winner && (
        <motion.div
          initial={{ scale: 0 }}
          animate={{ scale: 1 }}
          className="text-xl font-bold text-red-600 mt-4"
        >
          🎉 主獎得主: {winner} 🎉
        </motion.div>
      )}
      {bonusWinner && (
        <motion.div
          initial={{ scale: 0 }}
          animate={{ scale: 1 }}
          className="text-lg font-bold text-orange-600 mt-2"
        >
          🎊 加碼獎得主: {bonusWinner} 🎊
        </motion.div>
      )}
    </div>
  );
}
