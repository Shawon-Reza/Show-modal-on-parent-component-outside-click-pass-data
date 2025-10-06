# Show-modal-on-parent-component-outside-click-pass-data


To implement a reusable modal component in React that can be used within a parent component, you can follow this basic structure:

## Main Logic for Creating a Modal Component

### Create the Modal Component:
- Accept `isOpen` (boolean) and `onClose` (function) as props.
- Display the modal when `isOpen` is `true` and allow closing it using `onClose`.

### Modal Component (`Modal.js`)

```jsx
import React, { useEffect, useRef } from "react";

const Modal = ({ isOpen, onClose, children }) => {
  const modalRef = useRef(null);

  // Close the modal if clicking outside
  useEffect(() => {
    const handleClickOutside = (event) => {
      if (modalRef.current && !modalRef.current.contains(event.target)) {
        onClose(); // Close modal if clicked outside
      }
    };

    if (isOpen) {
      document.addEventListener("mousedown", handleClickOutside);
    }
    return () => {
      document.removeEventListener("mousedown", handleClickOutside); // Clean up
    };
  }, [isOpen, onClose]);

  if (!isOpen) return null; // Don't render if the modal is closed

  return (
    <div className="fixed inset-0 bg-black/30 flex items-center justify-center z-50">
      <div
        ref={modalRef}
        className="bg-white p-6 rounded-lg w-1/3"
      >
        {/* Modal Content */}
        {children}

        {/* Close button */}
        <button onClick={onClose} className="mt-4 bg-gray-300 py-2 px-4 rounded">
          Close
        </button>
      </div>
    </div>
  );
};

export default Modal;
```
### Using the Modal in a Parent Component (ParentComponent.js)
- Create state to manage modal visibility (isModalOpen).
- Pass the isModalOpen state and the setIsModalOpen function as isOpen and onClose props to the Modal component.

```jsx
import React, { useState } from "react";
import Modal from "./Modal";

const ParentComponent = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);

  const handleOpenModal = () => {
    setIsModalOpen(true); // Open modal
  };

  const handleCloseModal = () => {
    setIsModalOpen(false); // Close modal
  };

  return (
    <div>
      <button onClick={handleOpenModal} className="bg-blue-500 text-white py-2 px-4 rounded">
        Open Modal
      </button>

      {/* Modal Component */}
      <Modal isOpen={isModalOpen} onClose={handleCloseModal}>
        <h2>Modal Content</h2>
        <p>This is a reusable modal component.</p>
        {/* You can add more content here */}
      </Modal>
    </div>
  );
};

export default ParentComponent;

