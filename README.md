# agile_frontend
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sprint Status</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
        }
        .chart-container {
            width: 50%;
            margin: 20px auto;
        }
        .modal-body {
            padding: 20px;
        }
        .modal-body p {
            font-size: 16px;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <header>
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <a class="navbar-brand" href="#">Sprints Status</a>
        </nav>
    </header>
    <main>
        <div class="container">
            <div class="row">
                <div class="col-md-12">
                    <h1>Sprints Status</h1>
                    <div class="chart-container">
                        <canvas id="sprint-status-chart"></canvas>
                    </div>
                </div>
            </div>
        </div>
    </main>
    <div class="modal fade" id="sprintDetailsModal" tabindex="-1" role="dialog" aria-labelledby="sprintDetailsModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="sprintDetailsModalLabel">Sprint Details</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <p id="sprint-name"></p>
                    <p id="sprint-status"></p>
                    <p id="sprint-description"></p>
                </div>
            </div>
        </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@2.9.4/dist/Chart.min.js"></script>
    <script>
        var sprintStatusCtx = document.getElementById('sprint-status-chart').getContext('2d');
        var sprintDetails = {
            'Sprint 1': { status: 'Completed', description: 'This is the first sprint.', value: 100 },
            'Sprint 2': { status: 'Ongoing', description: 'This is the second sprint.', value: 50 },
            'Sprint 3': { status: 'Hold', description: 'This is the third sprint.', value: 20 },
            'Sprint 4': { status: 'Not Started', description: 'This is the fourth sprint.', value: 10 },
            'Sprint 5': { status: 'Upcoming', description: 'This is the fifth sprint.', value: 5 }
        };
        var sprintStatusData = {
            labels: Object.keys(sprintDetails),
            datasets: [{
                label: 'Sprint Status',
                data: Object.values(sprintDetails).map(detail => detail.value),
                backgroundColor: Object.values(sprintDetails).map(detail => {
                    switch (detail.status) {
                        case 'Completed':
                            return 'rgba(0, 128, 0, 0.2)';
                        case 'Ongoing':
                            return 'rgba(255, 255, 0, 0.2)';
                        case 'Hold':
                            return 'rgba(255, 0, 0, 0.2)';
                        case 'Not Started':
                            return 'rgba(128, 128, 128, 0.2)';
                        default:
                            return 'rgba(211, 211, 211, 0.2)';
                    }
                }),
                borderColor: Object.values(sprintDetails).map(detail => {
                    switch (detail.status) {
                        case 'Completed':
                            return 'rgba(0, 128, 0, 1)';
                        case 'Ongoing':
                            return 'rgba(255, 255, 0, 1)';
                        case 'Hold':
                            return 'rgba(255, 0, 0, 1)';
                        case 'Not Started':
                            return 'rgba(128, 128, 128, 1)';
                        default:
                            return 'rgba(211, 211, 211, 1)';
                    }
                }),
                borderWidth: 1
            }]
        };
        var sprintStatusOptions = {
            scales: {
                y: {
                    beginAtZero: true
                }
            },
            tooltips: {
                callbacks: {
                    label: function(tooltipItem, data) {
                        var sprintName = data.labels[tooltipItem.index];
                        var sprintStatus = sprintDetails[sprintName].status;
                        var sprintValue = sprintDetails[sprintName].value;
                        return sprintName + ': ' + sprintStatus + ' (' + sprintValue + ')';
                    }
                }
            },
            animation: {
                duration: 2000
            }
        };
        var sprintStatusChart = new Chart(sprintStatusCtx, {
            type: 'bar',
            data: sprintStatusData,
            options: sprintStatusOptions
        });
        sprintStatusCtx.canvas.onclick = function(event) {
            var activePoints = sprintStatusChart.getElementsAtEvent(event);
            if (activePoints.length > 0) {
                var sprintName = sprintStatusChart.data.labels[activePoints[0].index];
                var sprintStatus = sprintDetails[sprintName].status;
                var sprintDescription = sprintDetails[sprintName].description;
                $('#sprint-name').text(sprintName);
                $('#sprint-status').text('Status: ' + sprintStatus);
                $('#sprint-description').text('Description: ' + sprintDescription);
                $('#sprintDetailsModal').modal('show');
            }
        };
    </script>
</body>
</html>his is the front end work to show about the sprints status in agile project management .
