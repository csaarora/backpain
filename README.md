<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Clinical Pathway Tree Viewer</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f5f5f5;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .tree-item {
            margin: 5px 0;
            padding: 5px;
        }
        .toggle {
            cursor: pointer;
            user-select: none;
            color: #2563eb;
            margin-right: 5px;
        }
        .children {
            margin-left: 20px;
            border-left: 1px dashed #e5e7eb;
            padding-left: 10px;
        }
        .label {
            font-weight: 500;
        }
        .value {
            color: #4b5563;
        }
        .header {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 20px;
            color: #1e3a8a;
        }
        .node {
            padding: 4px 0;
        }
        .node:hover {
            background-color: #f3f4f6;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">Rural India Back Pain Management Pathway</div>
        <div id="tree"></div>
    </div>

    <script>
        const pathwayData = {
            "Initial Assessment": {
                "Red Flags": {
                    "Immediate Referral Triggers": [
                        {
                            "Symptom": "Fever",
                            "Context": "Unexplained"
                        },
                        {
                            "Symptom": "Weight Loss",
                            "Context": "Unexplained"
                        },
                        {
                            "Symptom": "Loss of Bowel/Bladder Control"
                        }
                    ],
                    "Action": "Refer to district hospital"
                },
                "Basic Assessment": {
                    "Required Elements": [
                        "Pain History",
                        "Work Activities",
                        "Home Responsibilities",
                        "Previous Treatments"
                    ]
                }
            },
            "Risk Stratification": {
                "Low Risk": {
                    "Criteria": [
                        "Pain < 6 weeks",
                        "No red flags",
                        "Able to perform daily activities with modification"
                    ]
                },
                "High Risk": {
                    "Criteria": [
                        "Pain > 6 weeks",
                        "Red flags present",
                        "Unable to perform daily activities"
                    ]
                }
            },
            "Management": {
                "Low Risk Cases": {
                    "Education": [
                        "Use local language",
                        "Visual aids",
                        "Include family members"
                    ],
                    "Activity Recommendations": [
                        "Avoid complete bed rest",
                        "Continue light activities",
                        "Modify essential tasks"
                    ],
                    "Pain Management": {
                        "First Line": [
                            "Local heat application",
                            "Simple exercises",
                            "Activity modification"
                        ],
                        "Second Line": [
                            "Paracetamol if available",
                            "NSAIDs if no contraindications",
                            "Safe traditional remedies"
                        ]
                    }
                }
            }
        };

        function createTreeNode(key, value) {
            const div = document.createElement('div');
            div.className = 'tree-item';

            if (typeof value === 'object' && value !== null) {
                const nodeDiv = document.createElement('div');
                nodeDiv.className = 'node';
                
                const toggle = document.createElement('span');
                toggle.className = 'toggle';
                toggle.textContent = '▼';
                
                const label = document.createElement('span');
                label.className = 'label';
                label.textContent = key;

                nodeDiv.appendChild(toggle);
                nodeDiv.appendChild(label);
                div.appendChild(nodeDiv);

                const childrenDiv = document.createElement('div');
                childrenDiv.className = 'children';

                if (Array.isArray(value)) {
                    value.forEach((item, index) => {
                        if (typeof item === 'object') {
                            Object.entries(item).forEach(([itemKey, itemValue]) => {
                                childrenDiv.appendChild(createTreeNode(itemKey, itemValue));
                            });
                        } else {
                            childrenDiv.appendChild(createTreeNode(`${index + 1}`, item));
                        }
                    });
                } else {
                    Object.entries(value).forEach(([childKey, childValue]) => {
                        childrenDiv.appendChild(createTreeNode(childKey, childValue));
                    });
                }

                div.appendChild(childrenDiv);

                toggle.onclick = () => {
                    childrenDiv.style.display = childrenDiv.style.display === 'none' ? 'block' : 'none';
                    toggle.textContent = childrenDiv.style.display === 'none' ? '▶' : '▼';
                };
            } else {
                const nodeDiv = document.createElement('div');
                nodeDiv.className = 'node';
                nodeDiv.innerHTML = `<span class="label">${key}:</span> <span class="value">${value}</span>`;
                div.appendChild(nodeDiv);
            }

            return div;
        }

        const treeContainer = document.getElementById('tree');
        Object.entries(pathwayData).forEach(([key, value]) => {
            treeContainer.appendChild(createTreeNode(key, value));
        });
    </script>
</body>
</html>
