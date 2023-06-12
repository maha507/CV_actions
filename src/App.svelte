
<script>
	import { onMount, createEventDispatcher } from "svelte";
	import "bootstrap/dist/css/bootstrap.min.css";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let gridData = [];
	let isCVUploadPopupVisible = false;
	let selectedRowData = null;
  
	async function uploadCV(file) {
	  // Perform further actions with the uploaded file
  
	  // Example: Update the backend API URL with the file upload
	  const formData = new FormData();
	  formData.append("file", file);
  
	  if (selectedRowData) {
		const uploadCandidateId = selectedRowData.id; // Get the candidate ID from selectedRowData
  
		try {
		  const response = await fetch(
			`https://api.recruitly.io/api/candidatecv/upload?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E&candidateId=${uploadCandidateId}`,
			{
			  method: "POST",
			  body: formData,
			}
		  );
  
		  if (response.ok) {
			console.log("CV uploaded successfully!");
			// Perform any additional actions upon successful upload
		  } else {
			console.error("CV upload failed.");
			// Handle the error accordingly
		  }
		} catch (error) {
		  console.error("CV upload error:", error);
		  // Handle the error accordingly
		}
	  }
  
	  // Close the CV upload popup
	  isCVUploadPopupVisible = false;
	}
  
	function handleSave() {
	  // Perform save logic
	  // In this case, we're updating the backend API URL in the handleSave function
	  console.log("Save clicked");
  
	  // Close the CV upload popup
	  isCVUploadPopupVisible = false;
	}
  
	function handleClose() {
	  // Perform close logic
	  console.log("Close clicked");
  
	  // Close the CV upload popup
	  isCVUploadPopupVisible = false;
	}
  
	const dispatch = createEventDispatcher();
  
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
			{
			  caption: "Actions",
			  width: 400,
			  cellTemplate: function (container, options) {
				const cvUploadButton = document.createElement("button");
				cvUploadButton.innerText = "CV Upload";
				cvUploadButton.classList.add("btn", "btn-success", "mr-2");
				cvUploadButton.addEventListener("click", function () {
				  const rowData = options.data;
				  selectedRowData = rowData;
				  isCVUploadPopupVisible = true;
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
			onSaveRowChanges: handleSave, // Bind handleSave function to the saveRowChanges event
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
  
  {#if isCVUploadPopupVisible}
  <div class="popup-overlay">
	<div class="popup-content">
	  <h3>Upload CV</h3>
	  <input
		type="file"
		on:change="{(event) => uploadCV(event.target.files[0])}"
		accept=".pdf,.doc,.docx"
	  />
	  <button on:click={handleSave} class="btn btn-primary">Save</button>
	  <button on:click={handleClose} class="btn btn-secondary">Close</button>
	</div>
  </div>
  {/if}
  
  <style>
  .popup-overlay {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background-color: rgba(0, 0, 0, 0.5);
	display: flex;
	justify-content: center;
	align-items: center;
  }
  
  .popup-content {
	background-color: white;
	padding: 20px;
	border-radius: 4px;
  }
  </style>
  