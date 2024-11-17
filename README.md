import React, { useState } from 'react';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Copy, Search } from 'lucide-react';
import { Textarea } from '@/components/ui/textarea';

const SimpleInventory = () => {
  const products = [
    "Age Defying 30 Sec Multi Action",
    "Avologi Eneo Advanced Gold",
    "Avologi Eneo Blue",
    "Avologi Eneo Classic Silver",
    "Avologi Eneo Duo Kit",
    "Cleansing Foam Gel",
    "ENEO TOTAL",
    "ENEO TOTAL BLU",
    "Exfoliating Body Scrub",
    "Facial Cleanser For Men",
    "Facial Cleansing Toner",
    "Instant 60 Seconds Express",
    "Lifting Facial Mask",
    "Lifting Moisturizing Cream",
    "L'Rae RF",
    "Luminice Classic",
    "Nail Kit - Gratiae",
    "Nourishing Cream For Men",
    "Perfume Men Gratiae",
    "Refreshing Moisturizing Gel Foam",
    "Renelif Eye Skin Therapy",
    "Renelif Neck Machine",
    "Renewing Facial Peeling Gel",
    "Renewing Moisturizing Cream",
    "Renewing Morning Reactivating",
    "Replenishing Eye Cream (No SPF)",
    "Replenishing Eye Serum (No SPF)",
    "Silky Body Butter",
    "Soin Blanc Cream",
    "Soin Blanc Mask",
    "Ultrox Facial Age Defying Cream",
    "Ultrox Facial Mask",
    "Ultrox Facial Serum"
  ];

  const getCurrentTime = () => {
    const now = new Date();
    return now.toTimeString().split(' ')[0].substr(0, 5); // Returns HH:MM format
  };

  const [quantities, setQuantities] = useState({});
  const [date, setDate] = useState(new Date().toISOString().split('T')[0]);
  const [time, setTime] = useState(getCurrentTime());
  const [searchTerm, setSearchTerm] = useState('');
  const [showCopyText, setShowCopyText] = useState(false);

  const handleQuantityChange = (product, value) => {
    setQuantities(prev => ({
      ...prev,
      [product]: value
    }));
  };

  const generateMessage = () => {
    let message = `Gratiae Inventory - ${date} ${time}\n\n`;
    products.forEach((product, index) => {
      if (quantities[product]) {
        message += `${index + 1}. ${product}: ${quantities[product]}\n`;
      }
    });
    return message;
  };

  const handleShowText = () => {
    setShowCopyText(true);
  };

  const handleCopyAll = () => {
    const textArea = document.createElement('textarea');
    textArea.value = generateMessage();
    document.body.appendChild(textArea);
    textArea.select();
    try {
      document.execCommand('copy');
      document.body.removeChild(textArea);
    } catch (err) {
      console.error('Failed to copy', err);
    }
  };

  const filteredProducts = products.filter(product =>
    product.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div className="max-w-4xl mx-auto p-4">
      <div className="mb-12 pt-4">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 100" className="w-full max-w-md mx-auto">
          <text x="50%" y="45" textAnchor="middle" style={{fontFamily: 'serif', fontSize: '48px', fill: '#B8860B'}}>GRATIAE</text>
          <text x="50%" y="70" textAnchor="middle" style={{fontFamily: 'sans-serif', fontSize: '14px', fill: '#B8860B', letterSpacing: '2px'}}>ORGANIC BEAUTY BY NATURE</text>
        </svg>
      </div>

      <div className="mb-6 space-y-4">
        <div className="flex gap-2 flex-wrap">
          <div className="flex gap-2">
            <Input
              type="date"
              value={date}
              onChange={(e) => setDate(e.target.value)}
              className="w-40 border-[#B8860B]"
            />
            <Input
              type="time"
              value={time}
              onChange={(e) => setTime(e.target.value)}
              className="w-32 border-[#B8860B]"
            />
          </div>
          <Button 
            onClick={handleShowText}
            className="bg-[#B8860B] hover:bg-[#986B0D] text-white"
          >
            <Copy className="w-4 h-4 mr-2" />
            Show Text
          </Button>
          <Button 
            onClick={handleCopyAll}
            className="bg-[#B8860B] hover:bg-[#986B0D] text-white"
          >
            <Copy className="w-4 h-4 mr-2" />
            Copy All
          </Button>
        </div>

        <div className="relative">
          <Search className="absolute left-2 top-2.5 h-4 w-4 text-[#B8860B]" />
          <Input
            placeholder="Search products..."
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            className="pl-8 border-[#B8860B]"
          />
        </div>

        {showCopyText && (
          <div className="p-4 border rounded border-[#B8860B] bg-gray-50">
            <div className="mb-2 font-bold text-[#B8860B]">Copy this text:</div>
            <Textarea
              value={generateMessage()}
              readOnly
              className="w-full h-40 bg-white border-[#B8860B]"
              onClick={(e) => e.target.select()}
            />
          </div>
        )}
      </div>

      <div className="space-y-2">
        {filteredProducts.map((product, index) => (
          <div key={product} className="flex items-center gap-2 border border-[#B8860B]/20 p-3 rounded hover:border-[#B8860B]/40 transition-colors">
            <div className="w-8 text-[#B8860B] font-medium">{index + 1}.</div>
            <div className="flex-1">{product}</div>
            <Input
              type="number"
              value={quantities[product] || ''}
              onChange={(e) => handleQuantityChange(product, e.target.value)}
              placeholder="Quantity"
              className="w-24 border-[#B8860B]"
            />
          </div>
        ))}
      </div>
    </div>
  );
};

export default SimpleInventory;
