import React, { useState } from 'react';

function TreeNode({ id, name, data, level = 0, setExpanded, selected, setSelected }) {
  const [expanded, setLocalExpanded] = useState(false);
  const children = data.filter(node => node.parentIds.includes(id));
  return (
    <>
      <div style={{ marginLeft: level * 20, backgroundColor : selected == id ? 'gray' : 'white' }}>
        <div onClick={() => { setLocalExpanded(!expanded); setSelected(id) }}>{name}</div>
        {expanded && (
          <div style={{ marginLeft: 20 }}>
            {children.map(node => (
              <TreeNode
                key={node.id}
                id={node.id}
                name={node.name}
                data={data}
                level={level + 1}
                setExpanded={setExpanded}
                selected={selected}
                setSelected={setSelected}
              />
            ))}
          </div>
        )}
      </div>
    </>
  );
}

function App() {
  const [expanded, setExpanded] = useState([]);
  const [selected,setSelected] = useState(null);
  const [records,setRecords] = useState([
    {
      "id": 1,
      "name": "Parent A",
      "parentIds": []
    },
    {
      "id": 2,
      "name": "Child A",
      "parentIds": [1]
    },
    {
      "id": 3,
      "name": "Child B",
      "parentIds": [1]
    },
    {
      "id": 4,
      "name": "Child C",
      "parentIds": [2,3]
    },
    {
      "id": 5,
      "name": "Child D",
      "parentIds": [2,3]
    },
    {
      "id": 6,
      "name": "Parent B",
      "parentIds": []
    },
    {
      "id": 7,
      "name": "Child E",
      "parentIds": [6]
    },
    {
      "id": 8,
      "name": "Child F",
      "parentIds": [6]
    }
  ]);
  
  return (
    <div>
      {records
        .filter(node => !node.parentIds.length)
        .map(node => (
          <TreeNode
            key={node.id}
            id={node.id}
            name={node.name}
            data={records}
            setExpanded={setExpanded}
            selected={selected}
            setSelected={setSelected}
          />
        ))}
    </div>
  );
}

export default App;
