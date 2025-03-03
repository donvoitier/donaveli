import { useState, useEffect } from "react";
import { motion } from "framer-motion";
import { Home, Gamepad2, Music, Film, Settings } from "lucide-react";

const categories = [
  { name: "Home", icon: <Home size={40} />, content: "Welcome to Don V Studios. Explore my work, aesthetics, and creative vision." },
  { name: "Games", icon: <Gamepad2 size={40} />, content: "Game concepts, animations, and experimental demos by Don V Studios." },
  { name: "Music", icon: <Music size={40} />, content: "Explore my music projects, sound direction, and influences." },
  { name: "Videos", icon: <Film size={40} />, content: "Cinematic projects, short films, and creative edits from Don V Studios." },
  { name: "Settings", icon: <Settings size={40} />, content: "Customize your experience and learn more about Don V Studios." },
];

export default function PS3Dashboard() {
  const [selectedIndex, setSelectedIndex] = useState(0);
  const [activePage, setActivePage] = useState(null);
  const [showIntro, setShowIntro] = useState(true);

  useEffect(() => {
    const timeout = setTimeout(() => setShowIntro(false), 3000);
    return () => clearTimeout(timeout);
  }, []);

  const handleKeyDown = (e) => {
    if (e.key === "ArrowLeft") {
      setSelectedIndex((prev) => (prev > 0 ? prev - 1 : categories.length - 1));
    } else if (e.key === "ArrowRight") {
      setSelectedIndex((prev) => (prev < categories.length - 1 ? prev + 1 : 0));
    }
  };

  if (showIntro) {
    return (
      <div className="flex h-screen w-full items-center justify-center bg-gray-900 text-white">
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ duration: 1.5 }}
          className="text-center"
        >
          <h1 className="text-5xl font-bold text-gray-400">Don V Studios</h1>
          <p className="mt-4 text-lg text-gray-500">Live, Create, Dominate.</p>
        </motion.div>
      </div>
    );
  }

  return (
    <div
      className="flex h-screen w-full flex-col items-center justify-center bg-gray-900 text-white"
      tabIndex={0}
      onKeyDown={handleKeyDown}
    >
      <div className={`flex ${activePage ? "absolute top-5" : "mb-8"} space-x-10`}>
        {categories.map((cat, index) => (
          <motion.div
            key={cat.name}
            animate={{ scale: index === selectedIndex ? 1.2 : 1 }}
            whileHover={{ scale: 1.4 }}
            transition={{ type: "spring", stiffness: 300, damping: 15 }}
            className={`flex flex-col items-center cursor-pointer transition-all duration-150 ${
              activePage && activePage.name === cat.name ? "text-gray-300" : "text-gray-500"
            }`}
            onClick={() => setActivePage(cat)}
          >
            {cat.icon}
            {!activePage && <span className="mt-2 text-lg">{cat.name}</span>}
          </motion.div>
        ))}
      </div>
      {activePage && (
        <motion.div
          key={activePage.name}
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ duration: 0.3 }}
          className="w-3/4 text-center mt-16"
        >
          <h1 className="text-4xl font-bold mb-4 text-gray-300">{activePage.name}</h1>
          <button 
            className="mt-6 px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700"
            onClick={() => setActivePage(null)}
          >
            Back to Menu
          </button>
        </motion.div>
      )}
    </div>
  );
}
