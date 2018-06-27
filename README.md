React 기반의 주소록
=====================
CRA 바탕
Redux, Immutable, PropTypes


localstorage 적용
-------------

#### src/modules/contacts.js
```
08: const LOAD_CONTACTS = 'contact/LOAD_CONTACTS';
14: export const loadContacts = createAction(LOAD_CONTACTS);
70: [LOAD_CONTACTS]: (state, action) => { return fromJS(action.payload); }
```

#### src/containers/ContactListContainer.js
```
12: componentDidUpdate(prevProps, prevState) {
        if(prevProps.contacts !== this.props.contacts) {
            localStorage.setItem('contacts', JSON.stringify(this.props.contacts));
        }
    }
58: export default connect(
        (state) => ({
            keyword: state.base.get('keyword'),
            contacts: state.contacts
        }),
        (dispatch) => ({
            ModalActions: bindActionCreators(modalActions, dispatch),
            ContactsActions: bindActionCreators(contactsActions, dispatch)
        })
    )(ContactListContainer);
```

#### src/App.js
```
17: componentDidMount() {
        const contacts = localStorage.getItem('contacts');
        if(!contacts) return;
        
        const { ContactsActions } = this.props;
        ContactsActions.loadContacts(JSON.parse(contacts));
    }
50: export default connect(
        (state) => ({
            view: state.base.get('view')
        }),
        (dispatch) => ({
            ContactsActions: bindActionCreators(contactsActions, dispatch)
        })
    )(App);
```
