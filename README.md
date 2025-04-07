// Updated UI Homepage Script
import React, { useState, useEffect } from 'react';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Card } from '@/components/ui/card';

const getCurrentTime = () => {
  const now = new Date();
  return now.toLocaleTimeString();
};

const getCurrentDate = () => {
  const now = new Date();
  return now.toLocaleDateString();
};

const Homepage = () => {
  const [zone2Visible, setZone2Visible] = useState(false);
  const [settingVisible, setSettingVisible] = useState(false);
  const [musicVisible, setMusicVisible] = useState(false);
  const [wallpaper, setWallpaper] = useState('');
  const [time, setTime] = useState(getCurrentTime());
  const [weather, setWeather] = useState('Berawan');
  const [date, setDate] = useState(getCurrentDate());
  const [bookmarks, setBookmarks] = useState([]);

  const [colorTheme, setColorTheme] = useState('#ffffff');

  useEffect(() => {
    const timer = setInterval(() => {
      setTime(getCurrentTime());
      setDate(getCurrentDate());
    }, 1000);
    return () => clearInterval(timer);
  }, []);

  const handleWallpaperChange = (e) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setWallpaper(reader.result);
        // Update color theme from wallpaper (mocked)
        setColorTheme('#ffeeee');
      };
      reader.readAsDataURL(file);
    }
  };

  const handleAddBookmark = () => {
    const url = prompt('Masukkan URL Bookmark:');
    if (url) {
      setBookmarks([...bookmarks, url]);
    }
  };

  const toggleZone2 = () => setZone2Visible(!zone2Visible);
  const toggleSettings = () => setSettingVisible(!settingVisible);
  const toggleMusic = () => setMusicVisible(!musicVisible);

  return (
    <div
      className="relative w-screen h-screen overflow-hidden"
      style={{ backgroundImage: `url(${wallpaper})`, backgroundSize: 'cover' }}
    >
      {/* Zona 1 */}
      <div className="absolute top-0 left-0 w-full h-full flex justify-end">
        <div className="flex flex-col items-end justify-between w-full h-full p-4">
          <div className="w-full flex justify-end">
            <button
              onClick={toggleZone2}
              className="bg-white/40 hover:bg-white/70 rounded px-3 py-1 shadow"
            >
              ||
            </button>
          </div>
          {!zone2Visible && (
            <button
              onClick={toggleSettings}
              className="absolute bottom-4 right-4 bg-white/50 hover:bg-white/80 rounded-full w-10 h-10 text-xl"
            >
              =
            </button>
          )}
        </div>
      </div>

      {/* Zona 2 */}
      {zone2Visible && (
        <div
          className="absolute right-0 top-0 h-full w-1/3 bg-white/60 backdrop-blur-lg p-4 overflow-hidden"
          style={{ background: `linear-gradient(to bottom, ${colorTheme}, #111111)` }}
        >
          <div className="flex justify-between items-center mb-2">
            <Input placeholder="Search Google..." className="w-full" />
          </div>
          <div className="text-sm flex flex-col gap-1 mb-4">
            <span>{time}</span>
            <span>{date}</span>
            <span>Cuaca: {weather}</span>
          </div>

          <div className="flex flex-wrap gap-2 overflow-y-auto max-h-40">
            {bookmarks.map((bm, index) => (
              <a
                key={index}
                href={bm}
                target="_blank"
                rel="noopener noreferrer"
                className="w-12 h-12 bg-white rounded shadow flex items-center justify-center overflow-hidden"
              >
                <img src={`https://www.google.com/s2/favicons?sz=64&domain_url=${bm}`} alt="icon" />
              </a>
            ))}
          </div>

          {musicVisible && (
            <div className="absolute bottom-4 left-4 right-4 bg-black rounded-md border-t-4 border-green-400 p-2 flex items-center justify-between">
              <div className="flex items-center gap-2">
                <div className="w-12 h-12 bg-white"></div>
                <span className="text-white">Now Playing</span>
              </div>
              <div className="flex items-center gap-2">
                <Button>⏮</Button>
                <Button>▶️</Button>
                <Button>🔁</Button>
              </div>
            </div>
          )}
        </div>
      )}

      {/* Setting bar = muncul di samping zona 2 */}
      {settingVisible && (
        <div className="absolute right-4 bottom-4 flex flex-col items-center gap-2 bg-white/50 p-2 rounded-md shadow">
          <button onClick={toggleMusic} className="hover:underline">🎵 Music</button>
          <label className="hover:underline cursor-pointer">
            🖼 Wallpaper
            <input type="file" onChange={handleWallpaperChange} hidden />
          </label>
          <button onClick={handleAddBookmark} className="hover:underline">📚 Bookmark</button>
        </div>
      )}

      {/* Google Logo */}
      <a
        href="https://www.google.com"
        target="_blank"
        rel="noopener noreferrer"
        className="absolute top-4 right-4"
      >
        <img
          src="https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png"
          alt="Google"
        />
      </a>
    </div>
  );
};
