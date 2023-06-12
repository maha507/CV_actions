<script>
	import { onMount } from "svelte";
	import "bootstrap/dist/css/bootstrap.min.css";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let gridData = [];
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
  
	  gridData = jsonData.map((item) => ({
		id: item.id,
		firstName: item.firstName,
		surname: item.surname,
		email: item.email,
		mobile: item.mobile,
	  }));
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			{ dataField: "id", caption: "ID", width: 50 },
			{ dataField: "firstName", caption: "First Name", width: 200 },
			{ dataField: "surname", caption: "Surname", width: 200 },
			{ dataField: "email", caption: "Email", width: 200 },
			{ dataField: "mobile", caption: "Mobile", width: 150 },
			// Define other columns as needed
			{
			  caption: "Actions",
			  width: 200,
			  cellTemplate: function (container, options) {
				const btnUpload = document.createElement("button");
				btnUpload.textContent = "CV Upload";
				btnUpload.className = "btn btn-primary btn-sm";
				btnUpload.addEventListener("click", function () {
				  // Implement CV Upload functionality
				  const candidateId = options.data.id;
				  // Implement CV Upload logic here
				  console.log("CV Upload for candidate ID:", candidateId);
				});
				container.appendChild(btnUpload);
  
				const btnDownload = document.createElement("button");
				btnDownload.textContent = "CV Download";
				btnDownload.className = "btn btn-success btn-sm";
				btnDownload.addEventListener("click", function () {
				  // Implement CV Download functionality
				  const candidateId = options.data.id;
				  // Implement CV Download logic here
				  console.log("CV Download for candidate ID:", candidateId);
				});
				container.appendChild(btnDownload);
  
				const btnView = document.createElement("button");
				btnView.textContent = "View CV";
				btnView.className = "btn btn-info btn-sm";
				btnView.addEventListener("click", function () {
				  // Implement View CV functionality
				  const candidateId = options.data.id;
				  // Implement View CV logic here
				  console.log("View CV for candidate ID:", candidateId);
				});
				container.appendChild(btnView);
			  },
			},
		  ],
		  showBorders: true,
		  filterRow: {
			visible: true,
		  },
		  editing: {
			allowDeleting: true,
			allowAdding: true,
			allowUpdating: true,
			mode: "popup",
			form: {
			  labelLocation: "top",
			},
			popup: {
			  showTitle: true,
			  title: "Row in the editing state",
			},
			texts: {
			  saveRowChanges: "Save",
			  cancelRowChanges: "Cancel",
			  deleteRow: "Delete",
			  confirmDeleteMessage:
				"Are you sure you want to delete this record?",
			},
		  },
		  paging: {
			pageSize: 20,
		  },
		  onRowInserting: async (e) => {
			// ...
		  },
		  onRowUpdating: async (e) => {
			// ...
		  },
		  onRowRemoving: async (e) => {
			// ...
		  },
		  onInitialized: () => {
			// Function called when the grid is initialized
			// ...
		  },
		}
	  );
	});
  </script>
  
  <div id="dataGrid"></div>
  