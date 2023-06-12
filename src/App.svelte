
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
		cvUrl: item.cvUrl, // assuming cvUrl is the property containing the CV file URL
	  }));
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(document.getElementById("dataGrid"), {
		dataSource: gridData,
		columns: [
		  { dataField: "id", caption: "ID", width: 50 },
		  { dataField: "firstName", caption: "firstName", width: 200 },
		  { dataField: "surname", caption: "surname", width: 200 },
		  { dataField: "email", caption: "Email", width: 200 },
		  { dataField: "mobile", caption: "mobile", width: 150 },
		  {
			caption: "Actions",
			width: 200,
			cellTemplate: function (container, options) {
			  const editButton = document.createElement("button");
			  editButton.innerText = "Edit";
			  editButton.classList.add("btn", "btn-primary", "mr-2");
			  editButton.addEventListener("click", function () {
				dataGrid.editRow(options.rowIndex);
			  });
  
			  const deleteButton = document.createElement("button");
			  deleteButton.innerText = "Delete";
			  deleteButton.classList.add("btn", "btn-danger");
			  deleteButton.addEventListener("click", function () {
				dataGrid.deleteRow(options.rowIndex);
			  });
  
			  const cvUploadButton = document.createElement("button");
			  cvUploadButton.innerText = "CV Upload";
			  cvUploadButton.classList.add("btn", "btn-success", "mr-2");
			  cvUploadButton.addEventListener("click", function () {
				const rowData = options.data;
				// Implement CV upload logic here
				console.log("CV Upload clicked for row:", rowData);
			  });
  
			  const cvDownloadButton = document.createElement("button");
			  cvDownloadButton.innerText = "CV Download";
			  cvDownloadButton.classList.add("btn", "btn-info", "mr-2");
			  cvDownloadButton.addEventListener("click", function () {
				const rowData = options.data;
				// Implement CV download logic here
				console.log("CV Download clicked for row:", rowData);
			  });
  
			  const viewCVButton = document.createElement("button");
			  viewCVButton.innerText = "View CV";
			  viewCVButton.classList.add("btn", "btn-secondary");
			  viewCVButton.addEventListener("click", function () {
				const rowData = options.data;
				// Implement view CV logic here
				console.log("View CV clicked for row:", rowData);
			  });
  
			  container.appendChild(editButton);
			  container.appendChild(deleteButton);
			  container.appendChild(cvUploadButton);
			  container.appendChild(cvDownloadButton);
			  container.appendChild(viewCVButton);
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
			confirmDeleteMessage: "Are you sure you want to delete this record?",
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
	  });
	});
  </script>
  
  <div id="dataGrid"></div>
  