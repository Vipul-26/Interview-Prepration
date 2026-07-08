import React, { useState, useEffect, useMemo } from 'react';

export default function App() {
  const [dataFromAPI, setDataFromAPI] = useState([]);
  const [loading, setLoading] = useState(false);
  const [searchedInput, setSearchedInput] = useState('');
  const [showList, setShowList] = useState(true);
  const [dobouncedSearch, setDebouncedSearch] = useState('');

  const mockData = [
    {
      id: 1,
      title: 'Apple',
    },
    {
      id: 2,
      title: 'Banana',
    },
    {
      id: 3,
      title: 'Orange',
    },
    {
      id: 4,
      title: 'Kiwi',
    },
    {
      id: 5,
      title: 'Guava',
    },
  ];

  const fetchSuggestions = () => {
    const newPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(mockData);
      }, 2000);
    });
    return newPromise;
  };

  useEffect(() => {
    setLoading(true);
    fetchSuggestions()
    .then((result) => {
      setDataFromAPI(result);
    })
    .finally(() => {
      setLoading(false);
    })
  }, []);

  useEffect(() => {
    let timer;
    timer = setTimeout(() => {
      setDebouncedSearch(searchedInput);
    }, 300);
    return () => clearTimeout(timer);
  }, [searchedInput]);

  const filteredSeachedData = useMemo(() => {
    return dataFromAPI.filter((data) => {
      return data.title.toLowerCase().includes(dobouncedSearch.toLowerCase());
    });
  }, [dataFromAPI, dobouncedSearch]);

  if (loading) {
    return <h1>Loading...</h1>;
  }

  return (
    <div>
      <input
        type="text"
        value={searchedInput}
        onChange={(e) => {
          setSearchedInput(e.target.value);
          setShowList(true);
        }}
      />
      {showList &&
        filteredSeachedData?.map((data) => {
          return (
           <div key={data.id}>
              <p
              onClick={() => {
                setSearchedInput(data.title);
                setShowList(false);
              }}
            >
              {data.title}
            </p>
            </div>
          );
        })}
    </div>
  );
}


export default App;
