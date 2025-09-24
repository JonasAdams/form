import React, { useState } from 'react';
import { Plus, Settings, Share, Eye, Edit3, Trash2, MapPin, Users, FileText } from 'lucide-react';

// Données géographiques de la RDC
const rdcData = {
  "Kinshasa": {
    "Kinshasa": {
      "Bandalungwa": ["Quarter 1", "Quarter 2", "Quarter 3"],
      "Barumbu": ["Barumbu Center", "Marché Central", "Port"],
      "Gombe": ["Centre-ville", "Résidentiel", "Ambassades"],
      "Kalamu": ["Matonge", "Victoire", "Salongo"],
      "Kasa-Vubu": ["Terminus", "Révolution", "Tshela"],
      "Kimbanseke": ["Kimbanseke Center", "Moluka", "Kisenso"],
      "Kinshasa": ["N'Djili", "Aéroport", "Kingabwa"],
      "Lemba": ["Lemba Center", "UPN", "Righini"],
      "Limete": ["Limete Center", "Kingabwa", "Résidentiel"],
      "Lingwala": ["Lingwala Center", "Marché", "Port"],
      "Makala": ["Makala Center", "BTP", "Industriel"],
      "Maluku": ["Maluku Center", "Nsele", "Mitendi"],
      "Masina": ["Masina Center", "Petro-Congo", "SOK"],
      "Matete": ["Matete Center", "Bisengo", "Kingabwa"],
      "Mont-Ngafula": ["Mbenseke", "Ngansele", "Kilambo"],
      "N'Djili": ["N'Djili Center", "Aéroport", "Cite"],
      "Ngaliema": ["Mont-Fleury", "Binza", "Ngaliema"],
      "N'Sele": ["N'Sele Center", "Kinkole", "Maluku"],
      "Bumbu": ["Bumbu Center", "Selembao", "Makala"],
      "Kintambo": ["Kintambo Center", "Ngaba", "Yolo"],
      "Ngaba": ["Ngaba Center", "Pakadjuma", "Righteous"],
      "Selembao": ["Selembao Center", "Matadi Kibala", "Cite"],
      "Bandalungwa": ["Pecheurs", "Socimat", "Quartier 8"],
      "Kasavubu": ["UPN", "Rightini", "Lemba"]
    }
  },
  "Kongo Central": {
    "Matadi": {
      "Matadi": ["Centre", "Port", "Nzanza", "Mvuzi"],
      "Nzanza": ["Nzanza Centre", "Plateau", "Bas-Fleuve"]
    },
    "Boma": {
      "Boma": ["Centre", "Port", "Kalamu", "Nkenge"],
      "Lukula": ["Lukula Centre", "Madimba", "Seke-Banza"]
    },
    "Mbanza-Ngungu": {
      "Mbanza-Ngungu": ["Centre", "Gare", "Nsioni", "Kivunda"],
      "Madimba": ["Madimba Centre", "Kimpese", "Songololo"]
    }
  },
  "Lualaba": {
    "Kolwezi": {
      "Kolwezi": ["Centre", "Dilala", "Manika", "Mutoshi"],
      "Manika": ["Manika Centre", "Lupoto", "Shinkolobwe"]
    },
    "Mutshatsha": {
      "Mutshatsha": ["Centre", "Kabolela", "Sandoa"],
      "Sandoa": ["Sandoa Centre", "Kaponda", "Lualaba"]
    }
  },
  "Haut-Katanga": {
    "Lubumbashi": {
      "Annex": ["Bel-Air", "Golf", "Residential"],
      "Kampemba": ["Kampemba Centre", "Bongonga", "Kasapa"],
      "Katuba": ["Katuba Centre", "Village", "Industriel"],
      "Kenya": ["Kenya Centre", "Tabacongo", "Gecamines"],
      "Lubumbashi": ["Centre-ville", "Gare", "Marché"],
      "Rwashi": ["Rwashi Centre", "Ruashi", "Mining"]
    },
    "Kipushi": {
      "Kipushi": ["Centre", "Frontière", "Mining", "Kasumbalesa"],
      "Kasumbalesa": ["Kasumbalesa Centre", "Douane", "Marché"]
    }
  },
  "Nord-Kivu": {
    "Goma": {
      "Goma": ["Centre-ville", "Lac Vert", "Volcans", "Aéroport"],
      "Karisimbi": ["Karisimbi Centre", "Ndosho", "Bujovu"],
      "Nyiragongo": ["Nyiragongo Centre", "Kibati", "Sake"]
    },
    "Butembo": {
      "Butembo": ["Centre", "Marché", "Université", "Mususa"],
      "Musienene": ["Musienene Centre", "Bulengera", "Kyondo"]
    }
  },
  "Sud-Kivu": {
    "Bukavu": {
      "Bukavu": ["Centre-ville", "Kadutu", "Bagira", "Ibanda"],
      "Kadutu": ["Kadutu Centre", "Nyawera", "Panzi"],
      "Bagira": ["Bagira Centre", "Kasha", "Nyalukemba"]
    },
    "Uvira": {
      "Uvira": ["Centre", "Port", "Sange", "Kiliba"],
      "Fizi": ["Fizi Centre", "Baraka", "Sebele"]
    }
  }
};

const FormBuilder = () => {
  const [forms, setForms] = useState([
    {
      id: 1,
      title: "Enquête Démographique Kinshasa",
      description: "Collecte de données sur la population de Kinshasa",
      responses: 45,
      status: "Actif",
      created: "2025-01-15"
    },
    {
      id: 2,
      title: "Inscription Événement Culturel",
      description: "Formulaire d'inscription pour l'événement culturel national",
      responses: 23,
      status: "Brouillon",
      created: "2025-01-10"
    }
  ]);

  const [selectedProvince, setSelectedProvince] = useState('');
  const [selectedVille, setSelectedVille] = useState('');
  const [selectedCommune, setSelectedCommune] = useState('');
  const [selectedQuartier, setSelectedQuartier] = useState('');
  
  const [showFormBuilder, setShowFormBuilder] = useState(false);
  const [showPreview, setShowPreview] = useState(false);
  const [currentForm, setCurrentForm] = useState({
    title: '',
    description: '',
    fields: []
  });

  const [formFields, setFormFields] = useState([
    { id: 1, type: 'text', label: 'Nom complet', required: true },
    { id: 2, type: 'email', label: 'Email', required: true },
    { id: 3, type: 'phone', label: 'Téléphone', required: false },
    { id: 4, type: 'location', label: 'Localisation', required: true }
  ]);

  const getVilles = () => {
    return selectedProvince ? Object.keys(rdcData[selectedProvince] || {}) : [];
  };

  const getCommunes = () => {
    return (selectedProvince && selectedVille) ? Object.keys(rdcData[selectedProvince][selectedVille] || {}) : [];
  };

  const getQuartiers = () => {
    return (selectedProvince && selectedVille && selectedCommune) ? 
      rdcData[selectedProvince][selectedVille][selectedCommune] || [] : [];
  };

  const addField = (type) => {
    const newField = {
      id: Date.now(),
      type,
      label: `Nouveau champ ${type}`,
      required: false
    };
    setFormFields([...formFields, newField]);
  };

  const removeField = (id) => {
    setFormFields(formFields.filter(field => field.id !== id));
  };

  const updateField = (id, updates) => {
    setFormFields(formFields.map(field => 
      field.id === id ? { ...field, ...updates } : field
    ));
  };

  const renderFieldPreview = (field) => {
    switch (field.type) {
      case 'text':
        return (
          <input 
            type="text" 
            placeholder={field.label}
            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        );
      case 'email':
        return (
          <input 
            type="email" 
            placeholder={field.label}
            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        );
      case 'phone':
        return (
          <input 
            type="tel" 
            placeholder="+243 XXX XXX XXX"
            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        );
      case 'location':
        return (
          <div className="space-y-3">
            <select 
              value={selectedProvince}
              onChange={(e) => {
                setSelectedProvince(e.target.value);
                setSelectedVille('');
                setSelectedCommune('');
                setSelectedQuartier('');
              }}
              className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            >
              <option value="">Sélectionnez la province</option>
              {Object.keys(rdcData).map(province => (
                <option key={province} value={province}>{province}</option>
              ))}
            </select>
            
            {selectedProvince && (
              <select 
                value={selectedVille}
                onChange={(e) => {
                  setSelectedVille(e.target.value);
                  setSelectedCommune('');
                  setSelectedQuartier('');
                }}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              >
                <option value="">Sélectionnez la ville</option>
                {getVilles().map(ville => (
                  <option key={ville} value={ville}>{ville}</option>
                ))}
              </select>
            )}
            
            {selectedVille && (
              <select 
                value={selectedCommune}
                onChange={(e) => {
                  setSelectedCommune(e.target.value);
                  setSelectedQuartier('');
                }}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              >
                <option value="">Sélectionnez la commune</option>
                {getCommunes().map(commune => (
                  <option key={commune} value={commune}>{commune}</option>
                ))}
              </select>
            )}
            
            {selectedCommune && (
              <select 
                value={selectedQuartier}
                onChange={(e) => setSelectedQuartier(e.target.value)}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              >
                <option value="">Sélectionnez le quartier</option>
                {getQuartiers().map(quartier => (
                  <option key={quartier} value={quartier}>{quartier}</option>
                ))}
              </select>
            )}
          </div>
        );
      case 'select':
        return (
          <select className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent">
            <option value="">Sélectionnez une option</option>
            <option value="option1">Option 1</option>
            <option value="option2">Option 2</option>
          </select>
        );
      case 'textarea':
        return (
          <textarea 
            rows="4"
            placeholder={field.label}
            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        );
      default:
        return <div className="p-3 bg-gray-100 rounded-lg">Champ non reconnu</div>;
    }
  };

  if (showPreview) {
    return (
      <div className="min-h-screen bg-gray-50">
        <div className="bg-white shadow-sm border-b">
          <div className="max-w-4xl mx-auto px-4 py-4">
            <div className="flex items-center justify-between">
              <h1 className="text-xl font-semibold text-gray-900">Aperçu du Formulaire</h1>
              <button
                onClick={() => setShowPreview(false)}
                className="px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition-colors"
              >
                Retour à l'éditeur
              </button>
            </div>
          </div>
        </div>

        <div className="max-w-2xl mx-auto p-6">
          <div className="bg-white rounded-xl shadow-sm border border-gray-200 p-8">
            <div className="mb-8">
              <h2 className="text-2xl font-bold text-gray-900 mb-2">
                {currentForm.title || "Titre du formulaire"}
              </h2>
              <p className="text-gray-600">
                {currentForm.description || "Description du formulaire"}
              </p>
            </div>

            <div className="space-y-6">
              {formFields.map(field => (
                <div key={field.id}>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    {field.label}
                    {field.required && <span className="text-red-500 ml-1">*</span>}
                  </label>
                  {renderFieldPreview(field)}
                </div>
              ))}
              
              <button
                className="w-full bg-blue-600 text-white py-3 px-6 rounded-lg font-medium hover:bg-blue-700 transition-colors"
                onClick={() => alert('Formulaire soumis avec succès!')}
              >
                Soumettre
              </button>
            </div>
          </div>
        </div>
      </div>
    );
  }

  if (showFormBuilder) {
    return (
      <div className="min-h-screen bg-gray-50">
        <div className="bg-white shadow-sm border-b">
          <div className="max-w-6xl mx-auto px-4 py-4">
            <div className="flex items-center justify-between">
              <h1 className="text-xl font-semibold text-gray-900">Créateur de Formulaires</h1>
              <div className="flex gap-3">
                <button
                  onClick={() => setShowPreview(true)}
                  className="flex items-center gap-2 px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors"
                >
                  <Eye className="w-4 h-4" />
                  Aperçu
                </button>
                <button
                  onClick={() => setShowFormBuilder(false)}
                  className="px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition-colors"
                >
                  Retour
                </button>
              </div>
            </div>
          </div>
        </div>

        <div className="max-w-6xl mx-auto p-6">
          <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
            {/* Panneau de configuration */}
            <div className="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
              <h3 className="text-lg font-semibold text-gray-900 mb-4">Configuration</h3>
              
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">Titre du formulaire</label>
                  <input
                    type="text"
                    value={currentForm.title}
                    onChange={(e) => setCurrentForm({...currentForm, title: e.target.value})}
                    placeholder="Enquête démographique..."
                    className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">Description</label>
                  <textarea
                    value={currentForm.description}
                    onChange={(e) => setCurrentForm({...currentForm, description: e.target.value})}
                    rows="3"
                    placeholder="Description de votre formulaire..."
                    className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
              </div>

              <div className="mt-6 pt-6 border-t border-gray-200">
                <h4 className="font-medium text-gray-900 mb-3">Ajouter des champs</h4>
                <div className="grid grid-cols-2 gap-2">
                  <button
                    onClick={() => addField('text')}
                    className="p-2 bg-blue-50 text-blue-600 rounded-lg hover:bg-blue-100 transition-colors text-sm"
                  >
                    Texte
                  </button>
                  <button
                    onClick={() => addField('email')}
                    className="p-2 bg-green-50 text-green-600 rounded-lg hover:bg-green-100 transition-colors text-sm"
                  >
                    Email
                  </button>
                  <button
                    onClick={() => addField('phone')}
                    className="p-2 bg-purple-50 text-purple-600 rounded-lg hover:bg-purple-100 transition-colors text-sm"
                  >
                    Téléphone
                  </button>
                  <button
                    onClick={() => addField('select')}
                    className="p-2 bg-orange-50 text-orange-600 rounded-lg hover:bg-orange-100 transition-colors text-sm"
                  >
                    Liste
                  </button>
                  <button
                    onClick={() => addField('textarea')}
                    className="p-2 bg-red-50 text-red-600 rounded-lg hover:bg-red-100 transition-colors text-sm"
                  >
                    Long texte
                  </button>
                  <button
                    onClick={() => addField('location')}
                    className="p-2 bg-yellow-50 text-yellow-600 rounded-lg hover:bg-yellow-100 transition-colors text-sm flex items-center justify-center gap-1"
                  >
                    <MapPin className="w-3 h-3" />
                    RDC
                  </button>
                </div>
              </div>
            </div>

            {/* Éditeur de champs */}
            <div className="lg:col-span-2 bg-white rounded-xl shadow-sm border border-gray-200 p-6">
              <h3 className="text-lg font-semibold text-gray-900 mb-4">Champs du formulaire</h3>
              
              <div className="space-y-4">
                {formFields.map((field, index) => (
                  <div key={field.id} className="border border-gray-200 rounded-lg p-4">
                    <div className="flex items-center justify-between mb-3">
                      <span className="text-sm font-medium text-gray-600">Champ #{index + 1}</span>
                      <button
                        onClick={() => removeField(field.id)}
                        className="text-red-500 hover:text-red-700 transition-colors"
                      >
                        <Trash2 className="w-4 h-4" />
                      </button>
                    </div>
                    
                    <div className="grid grid-cols-2 gap-3 mb-3">
                      <input
                        type="text"
                        value={field.label}
                        onChange={(e) => updateField(field.id, { label: e.target.value })}
                        placeholder="Libellé du champ"
                        className="p-2 border border-gray-300 rounded focus:ring-2 focus:ring-blue-500 focus:border-transparent text-sm"
                      />
                      <div className="flex items-center gap-2">
                        <input
                          type="checkbox"
                          checked={field.required}
                          onChange={(e) => updateField(field.id, { required: e.target.checked })}
                          className="rounded border-gray-300 text-blue-600 focus:ring-blue-500"
                        />
                        <label className="text-sm text-gray-700">Obligatoire</label>
                      </div>
                    </div>
                    
                    <div className="bg-gray-50 p-3 rounded">
                      {renderFieldPreview(field)}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  }

  // Dashboard principal
  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <div className="bg-white shadow-sm border-b">
        <div className="max-w-6xl mx-auto px-4 py-4">
          <div className="flex items-center justify-between">
            <div className="flex items-center gap-3">
              <div className="bg-blue-600 p-2 rounded-lg">
                <FileText className="w-6 h-6 text-white" />
              </div>
              <div>
                <h1 className="text-xl font-semibold text-gray-900">FormBuilder RDC</h1>
                <p className="text-sm text-gray-600">Créez des formulaires adaptés à la RDC</p>
              </div>
            </div>
            <button
              onClick={() => setShowFormBuilder(true)}
              className="flex items-center gap-2 bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors"
            >
              <Plus className="w-4 h-4" />
              Nouveau formulaire
            </button>
          </div>
        </div>
      </div>

      {/* Stats */}
      <div className="max-w-6xl mx-auto p-6">
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
          <div className="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
            <div className="flex items-center gap-4">
              <div className="bg-blue-100 p-3 rounded-lg">
                <FileText className="w-6 h-6 text-blue-600" />
              </div>
              <div>
                <h3 className="text-2xl font-bold text-gray-900">12</h3>
                <p className="text-gray-600">Formulaires créés</p>
              </div>
            </div>
          </div>
          
          <div className="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
            <div className="flex items-center gap-4">
              <div className="bg-green-100 p-3 rounded-lg">
                <Users className="w-6 h-6 text-green-600" />
              </div>
              <div>
                <h3 className="text-2xl font-bold text-gray-900">1,247</h3>
                <p className="text-gray-600">Réponses reçues</p>
              </div>
            </div>
          </div>
          
          <div className="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
            <div className="flex items-center gap-4">
              <div className="bg-purple-100 p-3 rounded-lg">
                <MapPin className="w-6 h-6 text-purple-600" />
              </div>
              <div>
                <h3 className="text-2xl font-bold text-gray-900">26</h3>
                <p className="text-gray-600">Provinces couvertes</p>
              </div>
            </div>
          </div>
        </div>

        {/* Liste des formulaires */}
        <div className="bg-white rounded-xl shadow-sm border border-gray-200">
          <div className="p-6 border-b border-gray-200">
            <h2 className="text-lg font-semibold text-gray-900">Mes formulaires</h2>
          </div>
          
          <div className="divide-y divide-gray-200">
            {forms.map(form => (
              <div key={form.id} className="p-6 hover:bg-gray-50 transition-colors">
                <div className="flex items-center justify-between">
                  <div className="flex-1">
                    <h3 className="text-lg font-medium text-gray-900 mb-1">{form.title}</h3>
                    <p className="text-gray-600 mb-2">{form.description}</p>
                    <div className="flex items-center gap-4 text-sm text-gray-500">
                      <span>{form.responses} réponses</span>
                      <span className={`px-2 py-1 rounded-full text-xs font-medium ${
                        form.status === 'Actif' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'
                      }`}>
                        {form.status}
                      </span>
                      <span>Créé le {form.created}</span>
                    </div>
                  </div>
                  
                  <div className="flex items-center gap-2">
                    <button className="p-2 text-gray-500 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition-colors">
                      <Eye className="w-4 h-4" />
                    </button>
                    <button className="p-2 text-gray-500 hover:text-green-600 hover:bg-green-50 rounded-lg transition-colors">
                      <Edit3 className="w-4 h-4" />
                    </button>
                    <button className="p-2 text-gray-500 hover:text-purple-600 hover:bg-purple-50 rounded-lg transition-colors">
                      <Share className="w-4 h-4" />
                    </button>
                    <button className="p-2 text-gray-500 hover:text-gray-700 hover:bg-gray-100 rounded-lg transition-colors">
                      <Settings className="w-4 h-4" />
                    </button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
};

export default FormBuilder;
